---
layout: page
title: NER Function
description: Function for NER in Gibbon
---

```python

import spacy
import pandas as pd
import collections

nlp = spacy.load('en_core_web_sm') # good idea to initialize here


gibbon_by_chapter = pd.read_csv('gibbon_text.csv').rename(columns={'Unnamed: 0':'chapter'})

gibbon_by_chapter

chapter_text = gibbon_by_chapter['StringText'][42]


def get_places(chapter_text):

    chapter_doc = nlp(chapter_text)

     
    
    place_freq = collections.defaultdict(int)
    for entity in chapter_doc.ents:
        if (entity.label_ == 'GPE') or (entity.label_ == 'LOC'):
            place_freq[entity.text] += 1 # the utility of defaultdict!
    place_freq = dict(place_freq)

    #to df
    place_freq_df = pd.DataFrame.from_dict(place_freq, orient='index').reset_index().rename(columns={'index':'place_name',0:'frequency'})


    places = pd.read_csv('places.csv')
    names = pd.read_csv('names.csv')

    def get_pleiades_id(term):
        """
        Iterates through all of the possible names in the names.csv file
        Returns None if no matched names
        """
        name_row = names.loc[names['attested_form'] == term]
        if len(name_row) == 1:
            return int(name_row.place_id.iloc[0])
        else:
            name_row = names.loc[names['romanized_form_1'] == term]
            if len(name_row) == 1:
                return int(name_row.place_id.iloc[0])
            else:
                name_row = names.loc[names['romanized_form_2'] == term]
                if len(name_row) == 1:
                    return int(name_row.place_id.iloc[0])
                else:
                    name_row = names.loc[names['romanized_form_3'] == term]
                    if len(name_row) == 1:
                        return int(name_row.place_id.iloc[0])
                    else:
                        return None

    place_freq_df['pleiades_id'] = place_freq_df['place_name'].apply(get_pleiades_id)
    place_freq_final = place_freq_df.dropna().reset_index(drop=True)


    def get_lat(pl_id):
        places_row = places.loc[places['id'] == pl_id]
        if len(places_row) == 1:
            return places_row.representative_latitude.iloc[0]
    
    def get_long(pl_id):
        places_row = places.loc[places['id'] == pl_id]
        if len(places_row) == 1:
            return places_row.representative_longitude.iloc[0]

    place_freq_final['lat'] = place_freq_final['pleiades_id'].apply(get_lat)
    place_freq_final['long'] = place_freq_final['pleiades_id'].apply(get_long)
    p = place_freq_final.sort_values(by = ['frequency'], ascending=False)

    return p



get_places(chapter_text).to_csv('gibb42.csv')
```


```python

```
