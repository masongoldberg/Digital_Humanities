---
layout: page
title: Finding Places in Text
description: This lesson uses spaCy to find place and person names in a text and understand them using visualizations. 
---

# Source

Grunewald, Susan, and Andrew Janco. “Finding Places in Text with the World Historical Gazeteer.” Programming Historian, February 11, 2022. https://programminghistorian.org/en/lessons/finding-places-world-historical-gazetteer. 

Note: The World Historical Gazetteer was down when I tried to access it, so I did not do the end of the lesson. 

# Reflection

In this Programming Historian lesson, I learned how to process a text to recognize people and places, as well as how to be able to visualize that data. Additionally, I learned how to use spaCy to look at the structure of a sentence and how it is able to parse sentences syntactically, which I thought was super interesting. This lesson was clearly most helpful for me in learning how to extract locations from a text and tie them to real-life coordinates and other databases of things to get valuable information about the places and people I might extract from a text. For my final project, I will be extracting location information from a huge corpus of text, so knowing how to do it and link it up to another database and gazetteer will be super helpful for me. It was interesting to me to look at the two different ways posed to extract location data, one of them being literally by matching known location names with the text, the other being an AI algorithm that tries to figure out what looks like a place and what doesn’t look like a place. It was also interesting to me that it was really easy to process text in German, a language I don’t understand, but still got all of the information out of it that I wanted to get because spaCy made it easy, so I’m way less scared of dealing with texts in languages I don’t know in the future. And while I think that the most obvious use of this lesson for the future will be in finding locations in my final project, it will also be super useful if I ever want to do linguistic analysis in the future (though I am sure there are probably better programs for that). 

# Code


```python
# importing languages

from spacy.lang.de import German
from spacy.lang.en import English

#test
nlp = German()
doc = nlp("Berlin ist eine Stadt in Deutschland.")
for token in doc:
    print(token.i, token.text)
```

    0 Berlin
    1 ist
    2 eine
    3 Stadt
    4 in
    5 Deutschland
    6 .



```python
#import gazetteer

from pathlib import Path

gazetteer = Path("gazetteer.txt").read_text()
gazetteer = gazetteer.split("\n")


```


```python
#learning how to match place names

from spacy.lang.de import German
from spacy.matcher import Matcher

nlp = German()

doc = nlp("Karl-Heinz Quade ist von März 1944 bis August 1948 im Lager 150 in Grjasowez interniert.")


matcher = Matcher(nlp.vocab)

#add place pattern to matcher
for place in gazetteer:
    pattern = [{'LOWER': place.lower()}]
    matcher.add(place, [pattern])


#find places
matches = matcher(doc)
for match_id, start, end in matches:
    print(start, end, doc[start:end].text)


#add lagers to matcher
pattern = [{'LOWER': 'lager'},  #the first token should be ‘lager’
           {'LIKE_NUM': True}] # the second token should be a number

# Add the pattern to the matcher
matcher.add("LAGER_PATTERN", [pattern])

matches = matcher(doc)
for match_id, start, end in matches:
    print(start, end, doc[start:end].text)

```

    13 14 Grjasowez
    10 12 Lager 150
    13 14 Grjasowez



```python
#find places names in places folder (places.txt)

for file in Path("places").iterdir():
    doc = nlp(file.read_text())
    matches = matcher(doc)
    for match_id, start, end in matches:
        print(file.name, start, end, doc[start:end].text)
```

    places.txt 16 17 Stalingrad
    places.txt 33 34 Workuta
    places.txt 35 36 Astrachan
    places.txt 65 66 Stalingrad
    places.txt 77 78 Stalingrad
    places.txt 104 105 Stalingrad
    places.txt 106 107 Saratow
    places.txt 180 181 Stalingrad
    places.txt 190 191 Jelabuga
    places.txt 193 194 Stalingrad
    places.txt 222 223 Stalingrad
    places.txt 240 241 Jelabuga
    places.txt 382 383 Stalingrad
    places.txt 396 397 Stalingrad
    places.txt 416 417 Stalingrad
    places.txt 456 457 Stalingrad
    places.txt 483 484 Stalingrad
    places.txt 521 522 Stalingrad
    places.txt 600 601 Moskau
    places.txt 649 650 Stalingrad
    places.txt 663 664 Stalingrad
    places.txt 715 716 Stalingrad
    places.txt 730 731 Stalingrad
    places.txt 794 795 Stalingrad
    places.txt 825 826 Russland
    places.txt 861 862 Stalingrad
    places.txt 880 881 Stalingrad
    places.txt 971 972 Stalingrad
    places.txt 1056 1057 Stalingrad
    places.txt 1081 1082 Stalingrad
    places.txt 1119 1120 Stalingrad
    places.txt 1140 1141 Stalingrad
    places.txt 1166 1167 Stalingrad
    places.txt 1186 1187 Stalingrad
    places.txt 1241 1242 Stalingrad
    places.txt 1275 1276 Stalingrad
    places.txt 1336 1337 Stalingrad
    places.txt 1456 1457 Stalingrad
    places.txt 1483 1484 Russland
    places.txt 1506 1507 Stalingrad
    places.txt 1563 1564 Stalingrad
    places.txt 1631 1632 Stalingrad
    places.txt 1682 1683 Stalingrad
    places.txt 1744 1745 Stalingrad
    places.txt 1769 1770 Russland
    places.txt 1795 1796 Stalingrad
    places.txt 1830 1831 Stalingrad
    places.txt 1919 1920 Stalingrad
    places.txt 1931 1932 Stalingrad
    places.txt 1940 1941 Russland
    places.txt 1949 1950 Stalingrad
    places.txt 1977 1978 Stalingrad
    places.txt 1985 1986 Stalingrad
    places.txt 2133 2134 Stalingrad
    places.txt 2364 2365 Keller
    places.txt 2484 2485 Stalingrad
    places.txt 2516 2517 Stalingrad
    places.txt 2558 2559 Gorki
    places.txt 2570 2571 Moskau
    places.txt 2592 2593 Stalingrad
    places.txt 2594 2595 Saratow
    places.txt 2596 2597 Wolsk
    places.txt 2598 2599 Sysran
    places.txt 2600 2601 Kuibyschew
    places.txt 2604 2605 Uljanowsk
    places.txt 2606 2607 Kasan
    places.txt 2610 2611 Kasan
    places.txt 2612 2613 Moskau
    places.txt 2714 2715 Kasan
    places.txt 2765 2766 Stalingrad
    places.txt 2834 2835 Stalingrad
    places.txt 3002 3003 Kasan
    places.txt 3039 3040 Kasan
    places.txt 3079 3080 Kasan
    places.txt 3113 3114 Tscheboksary
    places.txt 3139 3140 Tula
    places.txt 3193 3194 Russland
    places.txt 3259 3260 Moskau
    places.txt 3267 3268 Jelabuga
    places.txt 3520 3521 Kaluga
    places.txt 3529 3530 Tula
    places.txt 3553 3554 Moskau
    places.txt 3570 3571 Derbent
    places.txt 3574 3575 Baku
    places.txt 3587 3588 Astrachan
    places.txt 3592 3593 Moskau
    places.txt 3608 3609 Pugatschow
    places.txt 3626 3627 Moskau
    places.txt 3714 3715 Ural
    places.txt 3721 3722 Ural
    places.txt 3730 3731 Kasan
    places.txt 3733 3734 Pugatschow
    places.txt 3738 3739 Moskau
    places.txt 4066 4067 Moskau
    places.txt 4121 4122 Smolensk
    places.txt 4139 4140 Smolensk
    places.txt 4237 4238 Workuta
    places.txt 4239 4240 Astrachan
    places.txt 4311 4312 Smolensk
    places.txt 4323 4324 Sibirien
    places.txt 4466 4467 Smolensk
    places.txt 4494 4495 Smolensk
    places.txt 4568 4569 Smolensk
    places.txt 4596 4597 Johannes
    places.txt 4604 4605 Smolensk
    places.txt 4627 4628 Smolensk
    places.txt 5097 5098 Borowitschi
    places.txt 5121 5122 KALININ
    places.txt 5127 5128 Borissow
    places.txt 5130 5131 MINSK
    places.txt 5132 5133 Orscha
    places.txt 5138 5139 WITEBSK
    places.txt 5156 5157 LWOW
    places.txt 5167 5168 Stanislaw
    places.txt 5180 5181 Winniza
    places.txt 5182 5183 Tscherkassy
    places.txt 5184 5185 Molotowsk
    places.txt 5193 5194 SMOLENSK
    places.txt 5196 5197 MOSKAU
    places.txt 5200 5201 KALUGA
    places.txt 5217 5218 BRJANSK
    places.txt 5221 5222 Lebedjan
    places.txt 5224 5225 ARCHANGELSK
    places.txt 5227 5228 Temnikow
    places.txt 5230 5231 PENSA
    places.txt 5232 5233 Wolsk
    places.txt 5248 5249 Kineschma
    places.txt 5257 5258 Susdal
    places.txt 5263 5264 WLADIMIR
    places.txt 5294 5295 KURSK
    places.txt 5297 5298 Sumy
    places.txt 5312 5313 ODESSA
    places.txt 5319 5320 POLTAWA
    places.txt 5320 5321 CHARKOW
    places.txt 5325 5326 Kupjansk
    places.txt 5329 5330 Atkarsk
    places.txt 5331 5332 SARATOW
    places.txt 5333 5334 Urjupinsk
    places.txt 5335 5336 Lissitschansk
    places.txt 5341 5342 Rjasan
    places.txt 5346 5347 Potma
    places.txt 5348 5349 Saransk
    places.txt 5351 5352 Morschansk
    places.txt 5355 5356 Kramatorsk
    places.txt 5360 5361 MAKEJEWKA
    places.txt 5364 5365 STALINO
    places.txt 5370 5371 Melitopol
    places.txt 5373 5374 Taganrog
    places.txt 5381 5382 SIMFEROPOL
    places.txt 5394 5395 Armawir
    places.txt 5401 5402 Nowotscherkassk
    places.txt 5442 5443 Rustawi
    places.txt 5451 5452 TASCHKENT
    places.txt 5453 5454 Tschuama
    places.txt 5459 5460 Kokand
    places.txt 5467 5468 Begowat
    places.txt 5717 5718 Kertsch
    places.txt 5771 5772 Maikop
    places.txt 5878 5879 Baku
    places.txt 5944 5945 Stalingrad
    places.txt 5991 5992 Astrachan
    places.txt 6057 6058 Tichorezk
    places.txt 6082 6083 Stalingrad
    places.txt 6087 6088 Astrachan
    places.txt 6100 6101 Stalingrad
    places.txt 6108 6109 Astrachan
    places.txt 6188 6189 Baku
    places.txt 6297 6298 Kertsch
    places.txt 6343 6344 Kertsch
    places.txt 6440 6441 Leningrad
    places.txt 6497 6498 Krasnogorsk
    places.txt 6499 6500 Moskau
    places.txt 6622 6623 Moskau
    places.txt 6653 6654 Moskau
    places.txt 6671 6672 Moskau
    places.txt 6826 6827 Jelabuga
    places.txt 6884 6885 Engels
    places.txt 6951 6952 Engels
    places.txt 7051 7052 Moskau
    places.txt 7064 7065 Moskau
    places.txt 7074 7075 Moskau
    places.txt 7139 7140 Moskau
    places.txt 7154 7155 Moskau
    places.txt 7177 7178 Krasnogorsk
    places.txt 7201 7202 Moskau
    places.txt 7309 7310 Marx
    places.txt 7364 7365 Gorki
    places.txt 7425 7426 Nowgorod
    places.txt 7428 7429 Gorki
    places.txt 7448 7449 Wladimir
    places.txt 7472 7473 Kasan
    places.txt 7497 7498 Nowgorod
    places.txt 7525 7526 Nowgorod
    places.txt 7557 7558 Moskau
    places.txt 7620 7621 Susdal
    places.txt 7631 7632 Moskau
    places.txt 7638 7639 Jelabuga
    places.txt 7719 7720 Gorki
    places.txt 7722 7723 Moskau
    places.txt 7724 7725 Leningrad
    places.txt 7732 7733 RSFSR
    places.txt 7901 7902 Gorki
    places.txt 7949 7950 Nowgorod
    places.txt 7955 7956 Gorki
    places.txt 7985 7986 Gorki
    places.txt 7988 7989 Nowgorod
    places.txt 8044 8045 Nowgorod
    places.txt 8050 8051 Moskau
    places.txt 8093 8094 Gorki
    places.txt 8235 8236 Jelabuga
    places.txt 8468 8469 Jelabuga
    places.txt 8484 8485 Jelabuga
    places.txt 8518 8519 Jelabuga
    places.txt 8535 8536 Jelabuga
    places.txt 8561 8562 Selenodolsk
    places.txt 8567 8568 Kasan
    places.txt 8573 8574 Selenodolsk
    places.txt 8580 8581 Selenodolsk
    places.txt 8587 8588 Selenodolsk
    places.txt 8602 8603 Moskau
    places.txt 8604 8605 Jarzewo
    places.txt 8610 8611 Jarzewo
    places.txt 8623 8624 Charkow
    places.txt 8635 8636 Charkow
    places.txt 8637 8638 Ukraine
    places.txt 9831 9832 Ukraine
    places.txt 9841 9842 Litauen
    places.txt 9843 9844 Estland
    places.txt 9980 9981 Wladimir
    places.txt 10114 10115 Moskau
    places.txt 10129 10130 Leningrad
    places.txt 10319 10320 Ural
    places.txt 10325 10326 Sibirien
    places.txt 10404 10405 Stalingrad
    places.txt 10590 10591 RSFSR
    places.txt 10597 10598 Ukraine
    places.txt 10601 10602 Usbekistan
    places.txt 10603 10604 Kasachstan
    places.txt 10605 10606 Georgien
    places.txt 10609 10610 Litauen
    places.txt 10611 10612 Estland
    places.txt 10617 10618 Armenien
    places.txt 10623 10624 Lettland
    places.txt 10791 10792 Kyschtym
    places.txt 10795 10796 Tscheljabinsk
    places.txt 10803 10804 Beketowka
    places.txt 10834 10835 Orscha
    places.txt 10836 10837 Beketowka
    places.txt 10838 10839 Wolsk
    places.txt 10865 10866 Kasan
    places.txt 10867 10868 Stalingrad
    places.txt 10878 10879 Workuta
    places.txt 10893 10894 Moskau
    places.txt 10895 10896 Workuta
    places.txt 10948 10949 Riga
    places.txt 10969 10970 Suchumi
    places.txt 10981 10982 Nowoschachtinsk
    places.txt 10993 10994 Brjansk
    places.txt 11009 11010 Stalingrad
    places.txt 11045 11046 Ragnit
    places.txt 11047 11048 Sibirien
    places.txt 11066 11067 Orsk
    places.txt 11085 11086 Moskau
    places.txt 11087 11088 Krasnogorsk
    places.txt 11390 11391 Moskau
    places.txt 11505 11506 Moskau
    places.txt 11519 11520 Moskau
    places.txt 11598 11599 Moskau
    places.txt 11617 11618 Moskau
    places.txt 11642 11643 Moskau
    places.txt 11659 11660 Krjukowo
    places.txt 11661 11662 Kaschira
    places.txt 11671 11672 Moskau
    places.txt 11690 11691 Moskau
    places.txt 11753 11754 Moskau
    places.txt 11963 11964 Moskau
    places.txt 11976 11977 Ural
    places.txt 11981 11982 Moskau
    places.txt 12049 12050 Moskau
    places.txt 12367 12368 Ar
    places.txt 12400 12401 Stalingrad
    places.txt 12431 12432 Stalingrad
    places.txt 12514 12515 Keller
    places.txt 12632 12633 Stalingrad
    places.txt 12645 12646 Stalingrad
    places.txt 12787 12788 Stalingrad
    places.txt 12867 12868 Russland
    places.txt 13011 13012 STALINGRAD
    places.txt 13055 13056 Stalingrad
    places.txt 13288 13289 Nowotroizk
    places.txt 13290 13291 Maksai
    places.txt 13608 13609 Moskau
    places.txt 13694 13695 Stalingrad
    places.txt 13715 13716 Rshew
    places.txt 13748 13749 Bor
    places.txt 13754 13755 Smolensk
    places.txt 13767 13768 Rshew
    places.txt 13772 13773 Gshatsk
    places.txt 13783 13784 Brjansk
    places.txt 13785 13786 Orjol
    places.txt 13793 13794 Smolensk
    places.txt 13821 13822 Witebsk
    places.txt 13833 13834 Roslawl
    places.txt 13845 13846 Brjansk
    places.txt 13852 13853 Smolensk
    places.txt 13875 13876 Minsk
    places.txt 13963 13964 Jelabuga
    places.txt 13995 13996 Kasan
    places.txt 14013 14014 Selenodolsk
    places.txt 14036 14037 Jelabuga
    places.txt 14078 14079 Kasan
    places.txt 14093 14094 Kasan
    places.txt 14111 14112 Selenodolsk
    places.txt 14123 14124 Selenodolsk
    places.txt 14148 14149 Kasan
    places.txt 14197 14198 Kasan
    places.txt 14231 14232 Kasan
    places.txt 14246 14247 Kasan
    places.txt 14267 14268 Kasan
    places.txt 14321 14322 Kasan
    places.txt 14370 14371 Selenodolsk
    places.txt 14533 14534 Selenodolsk
    places.txt 14607 14608 Selenodolsk
    places.txt 14654 14655 Selenodolsk
    places.txt 14721 14722 Riga
    places.txt 14736 14737 Selenodolsk
    places.txt 14769 14770 Selenodolsk
    places.txt 14846 14847 Selenodolsk
    places.txt 14944 14945 Tiraspol
    places.txt 14994 14995 Nikolajew
    places.txt 15108 15109 Odessa
    places.txt 15149 15150 Nikolajew
    places.txt 15195 15196 Nikolajew
    places.txt 15237 15238 Nikolajew
    places.txt 15311 15312 Nikolajew
    places.txt 15419 15420 Odessa
    places.txt 15437 15438 Odessa
    places.txt 15490 15491 Nikolajew
    places.txt 15498 15499 Odessa
    places.txt 15506 15507 Odessa
    places.txt 15523 15524 Odessa
    places.txt 15534 15535 Krim
    places.txt 15547 15548 Odessa
    places.txt 15613 15614 Odessa
    places.txt 15618 15619 Krim
    places.txt 15639 15640 Sibirien
    places.txt 15653 15654 Krim
    places.txt 15667 15668 Krim
    places.txt 15685 15686 Siwasch
    places.txt 15689 15690 Kertsch
    places.txt 15956 15957 Selenodolsk
    places.txt 16021 16022 Selenodolsk
    places.txt 16150 16151 Selenodolsk
    places.txt 16201 16202 Selenodolsk
    places.txt 16257 16258 Selenodolsk
    places.txt 16291 16292 Selenodolsk
    places.txt 16407 16408 Selenodolsk
    places.txt 16512 16513 Jelabuga
    places.txt 16536 16537 Jelabuga
    places.txt 16550 16551 Jelabuga
    places.txt 16571 16572 Selenodolsk
    places.txt 16580 16581 Jelabuga
    places.txt 16598 16599 Selenodolsk
    places.txt 16611 16612 Selenodolsk
    places.txt 16628 16629 Moskau
    places.txt 16639 16640 Jarzewo
    places.txt 16652 16653 Merefa
    places.txt 16677 16678 Charkow
    places.txt 16779 16780 Jelabuga
    places.txt 16863 16864 Moskau
    places.txt 16865 16866 Workuta
    places.txt 17127 17128 Moskau
    places.txt 17136 17137 Lubjanka
    places.txt 17140 17141 Butyrka
    places.txt 17159 17160 Moskau
    places.txt 17162 17163 Workuta
    places.txt 17249 17250 Karaganda
    places.txt 17251 17252 Workuta
    places.txt 17256 17257 Karaganda
    places.txt 17269 17270 Workuta
    places.txt 17338 17339 Workuta
    places.txt 17477 17478 Moskau
    places.txt 17660 17661 Workuta
    places.txt 17684 17685 Stalingrad
    places.txt 17805 17806 Moskau
    places.txt 17810 17811 USA
    places.txt 17877 17878 Jelabuga
    places.txt 18119 18120 Gorodischtsche
    places.txt 18213 18214 Stalingrad
    places.txt 18243 18244 Stalingrad
    places.txt 18276 18277 Stalingrad
    places.txt 18331 18332 Kasan
    places.txt 18359 18360 Jelschanka
    places.txt 18428 18429 Jelschanka
    places.txt 18455 18456 Gorodischtsche
    places.txt 18471 18472 Jelschanka
    places.txt 18540 18541 Stalingrad
    places.txt 18836 18837 Johannes
    places.txt 18877 18878 Krasnodar
    places.txt 18882 18883 Charkow
    places.txt 18899 18900 Minsk
    places.txt 18904 18905 Kiew
    places.txt 18907 18908 Jar
    places.txt 18958 18959 Nikolajew
    places.txt 19020 19021 Johannes
    places.txt 19024 19025 Stalingrad
    places.txt 19030 19031 Stalingrad
    places.txt 19261 19262 Jelabuga
    places.txt 20017 20018 ar
    places.txt 20370 20371 Stalingrad
    places.txt 20411 20412 Stalingrad
    places.txt 20452 20453 Stalingrad
    places.txt 20529 20530 Stalingrad
    places.txt 20591 20592 Stalingrad
    places.txt 20640 20641 Stalingrad
    places.txt 20739 20740 Stalingrad
    places.txt 20761 20762 Stalingrad
    places.txt 20818 20819 Russland
    places.txt 20906 20907 Stalingrad
    places.txt 20941 20942 Russland
    places.txt 20963 20964 Stalingrad
    places.txt 21021 21022 Stalingrad
    places.txt 21069 21070 Jelabuga
    places.txt 21089 21090 Stalingrad
    places.txt 21233 21234 Stalingrad
    places.txt 21284 21285 Stalingrad
    places.txt 21897 21898 Adler
    places.txt 22069 22070 Adler
    places.txt 22083 22084 Adler
    places.txt 22253 22254 Kursk
    places.txt 22333 22334 Wjasma
    places.txt 22335 22336 Gshatsk
    places.txt 22339 22340 Smolensk
    places.txt 22350 22351 Kursk
    places.txt 22371 22372 Smolensk
    places.txt 22394 22395 Smolensk
    places.txt 22396 22397 Roslawl
    places.txt 22410 22411 Roslawl
    places.txt 22448 22449 Smolensk
    places.txt 22450 22451 Roslawl
    places.txt 22477 22478 Smolensk
    places.txt 22500 22501 Gomel
    places.txt 22570 22571 Witebsk
    places.txt 22737 22738 Witebsk
    places.txt 22739 22740 Borissow
    places.txt 22741 22742 Bobruisk
    places.txt 23209 23210 Moskau
    places.txt 23413 23414 Stalingrad
    places.txt 23506 23507 Stalingrad
    places.txt 23790 23791 Dwiri
    places.txt 23795 23796 Georgien
    places.txt 23996 23997 Jelabuga
    places.txt 24018 24019 Jelabuga
    places.txt 24077 24078 Stalingrad
    places.txt 24103 24104 Kasan
    places.txt 24173 24174 Jelabuga
    places.txt 24321 24322 Stalingrad
    places.txt 24329 24330 Stalingrad
    places.txt 24486 24487 Jelabuga
    places.txt 24529 24530 Jelabuga
    places.txt 24538 24539 Kasan
    places.txt 24932 24933 Stalingrad
    places.txt 24991 24992 Workuta
    places.txt 24993 24994 Astrachan
    places.txt 25029 25030 Stalingrad
    places.txt 25072 25073 Johannes
    places.txt 25605 25606 jelabuga
    places.txt 25638 25639 Selenodolsk
    places.txt 25670 25671 Jelabuga
    places.txt 25688 25689 Moskau
    places.txt 25690 25691 Jarzewo
    places.txt 25696 25697 Moskau
    places.txt 25702 25703 Minsk
    places.txt 25717 25718 Jarzewo
    places.txt 25763 25764 Merefa
    places.txt 25767 25768 Charkow
    places.txt 26099 26100 Jelabuga
    places.txt 26101 26102 Selenodolsk
    places.txt 26108 26109 Jelabuga
    places.txt 26391 26392 Stalingrad
    places.txt 26407 26408 Stalingrad
    places.txt 26440 26441 Stalingrad
    places.txt 26518 26519 Kasan
    places.txt 26533 26534 Stalingrad
    places.txt 26688 26689 Pensa
    places.txt 26694 26695 Pensa
    places.txt 26724 26725 Moskau
    places.txt 26737 26738 Pensa
    places.txt 26762 26763 Moskau
    places.txt 26825 26826 Moskau
    places.txt 26827 26828 Kuibyschew
    places.txt 26838 26839 Kuibyschew
    places.txt 26860 26861 Moskau
    places.txt 26898 26899 Stalingrad
    places.txt 26909 26910 Pensa
    places.txt 26911 26912 Kuibyschew
    places.txt 26990 26991 Jelabuga
    places.txt 27096 27097 Moskau
    places.txt 27160 27161 Jelabuga
    places.txt 27165 27166 Kasan
    places.txt 27167 27168 Pensa
    places.txt 27337 27338 Ukraine
    places.txt 27366 27367 Fastow
    places.txt 27368 27369 Shitomir
    places.txt 27372 27373 Owrutsch
    places.txt 27484 27485 Kursk
    places.txt 27486 27487 Orjol
    places.txt 27561 27562 Kiew
    places.txt 27575 27576 Kiew
    places.txt 27775 27776 Pronja
    places.txt 27804 27805 Gomel
    places.txt 27826 27827 Bobruisk
    places.txt 27837 27838 Moskau
    places.txt 27846 27847 Gomel
    places.txt 27853 27854 Retschiza
    places.txt 27864 27865 Shlobin
    places.txt 27866 27867 Mosyr
    places.txt 27885 27886 Retschiza
    places.txt 28076 28077 Krasnogorsk
    places.txt 28122 28123 Stalingrad
    places.txt 28126 28127 Kameschkowo
    places.txt 28184 28185 Stalingrad
    places.txt 28382 28383 Stalingrad
    places.txt 28397 28398 Stalingrad
    places.txt 28403 28404 Kameschkowo
    places.txt 28489 28490 Kasan
    places.txt 28504 28505 Kameschkowo
    places.txt 28659 28660 Stalingrad
    places.txt 28684 28685 STALINGRAD



```python
#create counter


from collections import Counter

count_list = []

for match_id, start, end in matches:
    count_list.append(doc[start:end].text)

counter = Counter(count_list)

for term, count in counter.most_common(10):
    print(term,count)

```

    Stalingrad 100
    Moskau 55
    Jelabuga 30
    Selenodolsk 26
    Kasan 25
    Smolensk 16
    Workuta 11
    Russland 8
    Gorki 8
    Odessa 8



```python
#using spacy for NER

import spacy
nlp = spacy.load("de_core_news_sm")

doc = nlp("Karl-Heinz Quade ist von März 1944 bis August 1948 im Lager 150 in Grjasowez interniert.")

#find entities in doc
for ent in doc.ents:
    print(ent.text, ent.label_, ent.start, ent.end)
```

    Karl-Heinz Quade PER 0 2
    Grjasowez LOC 13 14



```python
#using displaCy

from spacy import displacy


displacy.render(doc, jupyter=True, style="ent")

displacy.render(doc, jupyter=True, style="dep")


```


<span class="tex2jax_ignore"><div class="entities" style="line-height: 2.5; direction: ltr">
<mark class="entity" style="background: #ddd; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Karl-Heinz Quade
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">PER</span>
</mark>
 ist von März 1944 bis August 1948 im Lager 150 in 
<mark class="entity" style="background: #ff9561; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Grjasowez
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">LOC</span>
</mark>
 interniert.</div></span>



<span class="tex2jax_ignore"><svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xml:lang="de" id="4742a1c3a5f34888945546ca0f919242-0" class="displacy" width="2675" height="662.0" direction="ltr" style="max-width: none; height: 662.0px; color: #000000; background: #ffffff; font-family: Arial; direction: ltr">
<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="50">Karl-Heinz</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="50">PROPN</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="225">Quade</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="225">PROPN</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="400">ist</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="400">AUX</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="575">von</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="575">ADP</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="750">März</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="750">NOUN</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="925">1944</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="925">NUM</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="1100">bis</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1100">ADP</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="1275">August</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1275">NOUN</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="1450">1948</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1450">NUM</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="1625">im</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1625">ADP</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="1800">Lager</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1800">NOUN</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="1975">150</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1975">NUM</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="2150">in</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="2150">ADP</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="2325">Grjasowez</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="2325">PROPN</tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="572.0">
    <tspan class="displacy-word" fill="currentColor" x="2500">interniert.</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="2500">VERB</tspan>
</text>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-4742a1c3a5f34888945546ca0f919242-0-0" stroke-width="2px" d="M70,527.0 C70,439.5 200.0,439.5 200.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-4742a1c3a5f34888945546ca0f919242-0-0" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">pnc</textPath>
    </text>
    <path class="displacy-arrowhead" d="M70,529.0 L62,517.0 78,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-4742a1c3a5f34888945546ca0f919242-0-1" stroke-width="2px" d="M245,527.0 C245,439.5 375.0,439.5 375.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-4742a1c3a5f34888945546ca0f919242-0-1" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">sb</textPath>
    </text>
    <path class="displacy-arrowhead" d="M245,529.0 L237,517.0 253,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-4742a1c3a5f34888945546ca0f919242-0-2" stroke-width="2px" d="M595,527.0 C595,89.5 2495.0,89.5 2495.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-4742a1c3a5f34888945546ca0f919242-0-2" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">mo</textPath>
    </text>
    <path class="displacy-arrowhead" d="M595,529.0 L587,517.0 603,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-4742a1c3a5f34888945546ca0f919242-0-3" stroke-width="2px" d="M595,527.0 C595,439.5 725.0,439.5 725.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-4742a1c3a5f34888945546ca0f919242-0-3" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">nk</textPath>
    </text>
    <path class="displacy-arrowhead" d="M725.0,529.0 L733.0,517.0 717.0,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-4742a1c3a5f34888945546ca0f919242-0-4" stroke-width="2px" d="M770,527.0 C770,439.5 900.0,439.5 900.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-4742a1c3a5f34888945546ca0f919242-0-4" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">nk</textPath>
    </text>
    <path class="displacy-arrowhead" d="M900.0,529.0 L908.0,517.0 892.0,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-4742a1c3a5f34888945546ca0f919242-0-5" stroke-width="2px" d="M1120,527.0 C1120,177.0 2490.0,177.0 2490.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-4742a1c3a5f34888945546ca0f919242-0-5" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">mo</textPath>
    </text>
    <path class="displacy-arrowhead" d="M1120,529.0 L1112,517.0 1128,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-4742a1c3a5f34888945546ca0f919242-0-6" stroke-width="2px" d="M1120,527.0 C1120,439.5 1250.0,439.5 1250.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-4742a1c3a5f34888945546ca0f919242-0-6" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">nk</textPath>
    </text>
    <path class="displacy-arrowhead" d="M1250.0,529.0 L1258.0,517.0 1242.0,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-4742a1c3a5f34888945546ca0f919242-0-7" stroke-width="2px" d="M1295,527.0 C1295,439.5 1425.0,439.5 1425.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-4742a1c3a5f34888945546ca0f919242-0-7" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">nk</textPath>
    </text>
    <path class="displacy-arrowhead" d="M1425.0,529.0 L1433.0,517.0 1417.0,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-4742a1c3a5f34888945546ca0f919242-0-8" stroke-width="2px" d="M1645,527.0 C1645,264.5 2485.0,264.5 2485.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-4742a1c3a5f34888945546ca0f919242-0-8" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">mo</textPath>
    </text>
    <path class="displacy-arrowhead" d="M1645,529.0 L1637,517.0 1653,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-4742a1c3a5f34888945546ca0f919242-0-9" stroke-width="2px" d="M1645,527.0 C1645,439.5 1775.0,439.5 1775.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-4742a1c3a5f34888945546ca0f919242-0-9" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">nk</textPath>
    </text>
    <path class="displacy-arrowhead" d="M1775.0,529.0 L1783.0,517.0 1767.0,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-4742a1c3a5f34888945546ca0f919242-0-10" stroke-width="2px" d="M1820,527.0 C1820,439.5 1950.0,439.5 1950.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-4742a1c3a5f34888945546ca0f919242-0-10" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">nk</textPath>
    </text>
    <path class="displacy-arrowhead" d="M1950.0,529.0 L1958.0,517.0 1942.0,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-4742a1c3a5f34888945546ca0f919242-0-11" stroke-width="2px" d="M2170,527.0 C2170,352.0 2480.0,352.0 2480.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-4742a1c3a5f34888945546ca0f919242-0-11" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">mo</textPath>
    </text>
    <path class="displacy-arrowhead" d="M2170,529.0 L2162,517.0 2178,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-4742a1c3a5f34888945546ca0f919242-0-12" stroke-width="2px" d="M2170,527.0 C2170,439.5 2300.0,439.5 2300.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-4742a1c3a5f34888945546ca0f919242-0-12" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">nk</textPath>
    </text>
    <path class="displacy-arrowhead" d="M2300.0,529.0 L2308.0,517.0 2292.0,517.0" fill="currentColor"/>
</g>

<g class="displacy-arrow">
    <path class="displacy-arc" id="arrow-4742a1c3a5f34888945546ca0f919242-0-13" stroke-width="2px" d="M420,527.0 C420,2.0 2500.0,2.0 2500.0,527.0" fill="none" stroke="currentColor"/>
    <text dy="1.25em" style="font-size: 0.8em; letter-spacing: 1px">
        <textPath xlink:href="#arrow-4742a1c3a5f34888945546ca0f919242-0-13" class="displacy-label" startOffset="50%" side="left" fill="currentColor" text-anchor="middle">oc</textPath>
    </text>
    <path class="displacy-arrowhead" d="M2500.0,529.0 L2508.0,517.0 2492.0,517.0" fill="currentColor"/>
</g>
</svg></span>



```python
# named entity linking
import spacy_dbpedia_spotlight
nlp = spacy.load('de_core_news_sm')
nlp.add_pipe('dbpedia_spotlight', config={'language_code': 'de'})

doc = nlp("Karl-Heinz Quade ist von März 1944 bis August 1948 im Lager 150 in Grjasowez interniert.")
for ent in doc.ents:
    print(ent.text, ent.label_, ent.kb_id_)

#get info for Grjasowez

import requests
data = requests.get("http://de.dbpedia.org/data/Grjasowez.json").json()


```

    Grjasowez DBPEDIA_ENT http://de.dbpedia.org/resource/Grjasowez
    interniert DBPEDIA_ENT http://de.dbpedia.org/resource/Internierung



```python
#export

start_date = "1800" #YYYY-MM-DD
end_date = "2000"
source_title = "Karl-Heinz Quade Diary"

output_text = ""
column_header = "id\ttitle\ttitle_source\tstart\tend\n"  
output_text += column_header  

places_list = []
if matches:
    places_list.extend([ doc[start:end].text for match_id, start, end in matches ])
if doc.ents:
    places_list.extend([ ent.text for ent in doc.ents if ent.label_ == "GPE" or ent.label_ == "LOC"])

# remove duplicate place names by creating a list of names and then converting the list to a set
unique_places = set(places_list)

for id, place in enumerate(unique_places):
    output_text += f"{id}\t{place}\t{source_title}\t{start_date}\t{end_date}\n"

filename = source_title.lower().replace(' ','_') + '.tsv'
Path(filename).write_text(output_text)
print('created: ', filename)


```

    created:  karl-heinz_quade_diary.tsv



```python

```
