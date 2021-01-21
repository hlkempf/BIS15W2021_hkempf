---
title: "Lab 5 Homework"
author: "Hannah Kempf"
date: "2021-01-21"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Learning Goals  

*At the end of this exercise, you will be able to:*    

1. Develop your dplyr superpowers so you can easily and confidently manipulate dataframes.  

2. Learn helpful new functions that are part of the `janitor` package.  

## Instructions

## Load the tidyverse

```r
library("tidyverse")
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.0 ──
```

```
## ✓ ggplot2 3.3.3     ✓ purrr   0.3.4
## ✓ tibble  3.0.4     ✓ dplyr   1.0.2
## ✓ tidyr   1.1.2     ✓ stringr 1.4.0
## ✓ readr   1.4.0     ✓ forcats 0.5.0
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

## Load the superhero data
These are data taken from comic books and assembled by fans. The include a good mix of categorical and continuous data.  Data taken from: https://www.kaggle.com/claudiodavi/superhero-set  

Check out the way I am loading these data. If I know there are NAs, I can take care of them at the beginning. But, we should do this very cautiously. At times it is better to keep the original columns and data intact.  


```r
superhero_info <- readr::read_csv("data/heroes_information.csv", na = c("", "-99", "-"))
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   name = col_character(),
##   Gender = col_character(),
##   `Eye color` = col_character(),
##   Race = col_character(),
##   `Hair color` = col_character(),
##   Height = col_double(),
##   Publisher = col_character(),
##   `Skin color` = col_character(),
##   Alignment = col_character(),
##   Weight = col_double()
## )
```

```r
#repleaces the things after the "c" with a "na"

superhero_powers <- readr::read_csv("data/super_hero_powers.csv", na = c("", "-99", "-"))
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   .default = col_logical(),
##   hero_names = col_character()
## )
## ℹ Use `spec()` for the full column specifications.
```

## Data tidy
1. Some of the names used in the `superhero_info` data are problematic so you should rename them here.  

```r
names(superhero_info)
```

```
##  [1] "name"       "Gender"     "Eye color"  "Race"       "Hair color"
##  [6] "Height"     "Publisher"  "Skin color" "Alignment"  "Weight"
```

```r
superhero_info <- rename(superhero_info, gender = "Gender", eye_color = "Eye color", race= "Race", hair_color = "Hair color", height = "Height", publisher= "Publisher", skin_color= "Skin color", alignment="Alignment", weight = "Weight")

names(superhero_info)
```

```
##  [1] "name"       "gender"     "eye_color"  "race"       "hair_color"
##  [6] "height"     "publisher"  "skin_color" "alignment"  "weight"
```

Yikes! `superhero_powers` has a lot of variables that are poorly named. We need some R superpowers...

```r
head(superhero_powers)
```

```
## # A tibble: 6 x 168
##   hero_names Agility `Accelerated He… `Lantern Power … `Dimensional Aw…
##   <chr>      <lgl>   <lgl>            <lgl>            <lgl>           
## 1 3-D Man    TRUE    FALSE            FALSE            FALSE           
## 2 A-Bomb     FALSE   TRUE             FALSE            FALSE           
## 3 Abe Sapien TRUE    TRUE             FALSE            FALSE           
## 4 Abin Sur   FALSE   FALSE            TRUE             FALSE           
## 5 Abominati… FALSE   TRUE             FALSE            FALSE           
## 6 Abraxas    FALSE   FALSE            FALSE            TRUE            
## # … with 163 more variables: `Cold Resistance` <lgl>, Durability <lgl>,
## #   Stealth <lgl>, `Energy Absorption` <lgl>, Flight <lgl>, `Danger
## #   Sense` <lgl>, `Underwater breathing` <lgl>, Marksmanship <lgl>, `Weapons
## #   Master` <lgl>, `Power Augmentation` <lgl>, `Animal Attributes` <lgl>,
## #   Longevity <lgl>, Intelligence <lgl>, `Super Strength` <lgl>,
## #   Cryokinesis <lgl>, Telepathy <lgl>, `Energy Armor` <lgl>, `Energy
## #   Blasts` <lgl>, Duplication <lgl>, `Size Changing` <lgl>, `Density
## #   Control` <lgl>, Stamina <lgl>, `Astral Travel` <lgl>, `Audio
## #   Control` <lgl>, Dexterity <lgl>, Omnitrix <lgl>, `Super Speed` <lgl>,
## #   Possession <lgl>, `Animal Oriented Powers` <lgl>, `Weapon-based
## #   Powers` <lgl>, Electrokinesis <lgl>, `Darkforce Manipulation` <lgl>, `Death
## #   Touch` <lgl>, Teleportation <lgl>, `Enhanced Senses` <lgl>,
## #   Telekinesis <lgl>, `Energy Beams` <lgl>, Magic <lgl>, Hyperkinesis <lgl>,
## #   Jump <lgl>, Clairvoyance <lgl>, `Dimensional Travel` <lgl>, `Power
## #   Sense` <lgl>, Shapeshifting <lgl>, `Peak Human Condition` <lgl>,
## #   Immortality <lgl>, Camouflage <lgl>, `Element Control` <lgl>,
## #   Phasing <lgl>, `Astral Projection` <lgl>, `Electrical Transport` <lgl>,
## #   `Fire Control` <lgl>, Projection <lgl>, Summoning <lgl>, `Enhanced
## #   Memory` <lgl>, Reflexes <lgl>, Invulnerability <lgl>, `Energy
## #   Constructs` <lgl>, `Force Fields` <lgl>, `Self-Sustenance` <lgl>,
## #   `Anti-Gravity` <lgl>, Empathy <lgl>, `Power Nullifier` <lgl>, `Radiation
## #   Control` <lgl>, `Psionic Powers` <lgl>, Elasticity <lgl>, `Substance
## #   Secretion` <lgl>, `Elemental Transmogrification` <lgl>,
## #   `Technopath/Cyberpath` <lgl>, `Photographic Reflexes` <lgl>, `Seismic
## #   Power` <lgl>, Animation <lgl>, Precognition <lgl>, `Mind Control` <lgl>,
## #   `Fire Resistance` <lgl>, `Power Absorption` <lgl>, `Enhanced
## #   Hearing` <lgl>, `Nova Force` <lgl>, Insanity <lgl>, Hypnokinesis <lgl>,
## #   `Animal Control` <lgl>, `Natural Armor` <lgl>, Intangibility <lgl>,
## #   `Enhanced Sight` <lgl>, `Molecular Manipulation` <lgl>, `Heat
## #   Generation` <lgl>, Adaptation <lgl>, Gliding <lgl>, `Power Suit` <lgl>,
## #   `Mind Blast` <lgl>, `Probability Manipulation` <lgl>, `Gravity
## #   Control` <lgl>, Regeneration <lgl>, `Light Control` <lgl>,
## #   Echolocation <lgl>, Levitation <lgl>, `Toxin and Disease Control` <lgl>,
## #   Banish <lgl>, `Energy Manipulation` <lgl>, `Heat Resistance` <lgl>, …
```

## `janitor`
The [janitor](https://garthtarr.github.io/meatR/janitor.html) package is your friend. Make sure to install it and then load the library.  


```r
library("janitor")
```

```
## 
## Attaching package: 'janitor'
```

```
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

The `clean_names` function takes care of everything in one line! Now that's a superpower!


```r
superhero_powers <- janitor::clean_names(superhero_powers)

head(superhero_powers)
```

```
## # A tibble: 6 x 168
##   hero_names agility accelerated_hea… lantern_power_r… dimensional_awa…
##   <chr>      <lgl>   <lgl>            <lgl>            <lgl>           
## 1 3-D Man    TRUE    FALSE            FALSE            FALSE           
## 2 A-Bomb     FALSE   TRUE             FALSE            FALSE           
## 3 Abe Sapien TRUE    TRUE             FALSE            FALSE           
## 4 Abin Sur   FALSE   FALSE            TRUE             FALSE           
## 5 Abominati… FALSE   TRUE             FALSE            FALSE           
## 6 Abraxas    FALSE   FALSE            FALSE            TRUE            
## # … with 163 more variables: cold_resistance <lgl>, durability <lgl>,
## #   stealth <lgl>, energy_absorption <lgl>, flight <lgl>, danger_sense <lgl>,
## #   underwater_breathing <lgl>, marksmanship <lgl>, weapons_master <lgl>,
## #   power_augmentation <lgl>, animal_attributes <lgl>, longevity <lgl>,
## #   intelligence <lgl>, super_strength <lgl>, cryokinesis <lgl>,
## #   telepathy <lgl>, energy_armor <lgl>, energy_blasts <lgl>,
## #   duplication <lgl>, size_changing <lgl>, density_control <lgl>,
## #   stamina <lgl>, astral_travel <lgl>, audio_control <lgl>, dexterity <lgl>,
## #   omnitrix <lgl>, super_speed <lgl>, possession <lgl>,
## #   animal_oriented_powers <lgl>, weapon_based_powers <lgl>,
## #   electrokinesis <lgl>, darkforce_manipulation <lgl>, death_touch <lgl>,
## #   teleportation <lgl>, enhanced_senses <lgl>, telekinesis <lgl>,
## #   energy_beams <lgl>, magic <lgl>, hyperkinesis <lgl>, jump <lgl>,
## #   clairvoyance <lgl>, dimensional_travel <lgl>, power_sense <lgl>,
## #   shapeshifting <lgl>, peak_human_condition <lgl>, immortality <lgl>,
## #   camouflage <lgl>, element_control <lgl>, phasing <lgl>,
## #   astral_projection <lgl>, electrical_transport <lgl>, fire_control <lgl>,
## #   projection <lgl>, summoning <lgl>, enhanced_memory <lgl>, reflexes <lgl>,
## #   invulnerability <lgl>, energy_constructs <lgl>, force_fields <lgl>,
## #   self_sustenance <lgl>, anti_gravity <lgl>, empathy <lgl>,
## #   power_nullifier <lgl>, radiation_control <lgl>, psionic_powers <lgl>,
## #   elasticity <lgl>, substance_secretion <lgl>,
## #   elemental_transmogrification <lgl>, technopath_cyberpath <lgl>,
## #   photographic_reflexes <lgl>, seismic_power <lgl>, animation <lgl>,
## #   precognition <lgl>, mind_control <lgl>, fire_resistance <lgl>,
## #   power_absorption <lgl>, enhanced_hearing <lgl>, nova_force <lgl>,
## #   insanity <lgl>, hypnokinesis <lgl>, animal_control <lgl>,
## #   natural_armor <lgl>, intangibility <lgl>, enhanced_sight <lgl>,
## #   molecular_manipulation <lgl>, heat_generation <lgl>, adaptation <lgl>,
## #   gliding <lgl>, power_suit <lgl>, mind_blast <lgl>,
## #   probability_manipulation <lgl>, gravity_control <lgl>, regeneration <lgl>,
## #   light_control <lgl>, echolocation <lgl>, levitation <lgl>,
## #   toxin_and_disease_control <lgl>, banish <lgl>, energy_manipulation <lgl>,
## #   heat_resistance <lgl>, …
```

```r
#this is wonderful.
```

## `tabyl`
The `janitor` package has many awesome functions that we will explore. Here is its version of `table` which not only produces counts but also percentages. Very handy! Let's use it to explore the proportion of good guys and bad guys in the `superhero_info` data.  


```r
tabyl(superhero_info, alignment)
```

```
##  alignment   n     percent valid_percent
##        bad 207 0.282016349    0.28473177
##       good 496 0.675749319    0.68225585
##    neutral  24 0.032697548    0.03301238
##       <NA>   7 0.009536785            NA
```

2. Notice that we have some neutral superheros! Who are they?


```r
superhero_info %>% 
  select(alignment, name) %>% 
  filter(alignment=="neutral")
```

```
## # A tibble: 24 x 2
##    alignment name        
##    <chr>     <chr>       
##  1 neutral   Bizarro     
##  2 neutral   Black Flash 
##  3 neutral   Captain Cold
##  4 neutral   Copycat     
##  5 neutral   Deadpool    
##  6 neutral   Deathstroke 
##  7 neutral   Etrigan     
##  8 neutral   Galactus    
##  9 neutral   Gladiator   
## 10 neutral   Indigo      
## # … with 14 more rows
```

## `superhero_info`

3. Let's say we are only interested in the variables name, alignment, and "race". How would you isolate these variables from `superhero_info`?


```r
names(superhero_info)
```

```
##  [1] "name"       "gender"     "eye_color"  "race"       "hair_color"
##  [6] "height"     "publisher"  "skin_color" "alignment"  "weight"
```


```r
superhero_info %>% 
  select(name, alignment, race)
```

```
## # A tibble: 734 x 3
##    name          alignment race             
##    <chr>         <chr>     <chr>            
##  1 A-Bomb        good      Human            
##  2 Abe Sapien    good      Icthyo Sapien    
##  3 Abin Sur      good      Ungaran          
##  4 Abomination   bad       Human / Radiation
##  5 Abraxas       bad       Cosmic Entity    
##  6 Absorbing Man bad       Human            
##  7 Adam Monroe   good      <NA>             
##  8 Adam Strange  good      Human            
##  9 Agent 13      good      <NA>             
## 10 Agent Bob     good      Human            
## # … with 724 more rows
```

## Not Human
4. List all of the superheros that are not human.


```r
superhero_info %>% 
  select(name, alignment, race) %>%
  filter(race != "Human")
```

```
## # A tibble: 222 x 3
##    name         alignment race             
##    <chr>        <chr>     <chr>            
##  1 Abe Sapien   good      Icthyo Sapien    
##  2 Abin Sur     good      Ungaran          
##  3 Abomination  bad       Human / Radiation
##  4 Abraxas      bad       Cosmic Entity    
##  5 Ajax         bad       Cyborg           
##  6 Alien        bad       Xenomorph XX121  
##  7 Amazo        bad       Android          
##  8 Angel        good      Vampire          
##  9 Angel Dust   good      Mutant           
## 10 Anti-Monitor bad       God / Eternal    
## # … with 212 more rows
```

## Good and Evil
5. Let's make two different data frames, one focused on the "good guys" and another focused on the "bad guys".

```r
good_guys <-
  superhero_info %>%
   select(name, alignment, race) %>%
  filter(alignment == "good")

bad_guys <-
  superhero_info %>%
  select(name, alignment, race) %>%
  filter(alignment == "bad")
```

6. For the good guys, use the `tabyl` function to summarize their "race".

```r
tabyl(good_guys, race)
```

```
##               race   n     percent valid_percent
##              Alien   3 0.006048387   0.010752688
##              Alpha   5 0.010080645   0.017921147
##             Amazon   2 0.004032258   0.007168459
##            Android   4 0.008064516   0.014336918
##             Animal   2 0.004032258   0.007168459
##          Asgardian   3 0.006048387   0.010752688
##          Atlantean   4 0.008064516   0.014336918
##         Bolovaxian   1 0.002016129   0.003584229
##              Clone   1 0.002016129   0.003584229
##             Cyborg   3 0.006048387   0.010752688
##           Demi-God   2 0.004032258   0.007168459
##              Demon   3 0.006048387   0.010752688
##            Eternal   1 0.002016129   0.003584229
##     Flora Colossus   1 0.002016129   0.003584229
##        Frost Giant   1 0.002016129   0.003584229
##      God / Eternal   6 0.012096774   0.021505376
##             Gungan   1 0.002016129   0.003584229
##              Human 148 0.298387097   0.530465950
##    Human / Altered   2 0.004032258   0.007168459
##     Human / Cosmic   2 0.004032258   0.007168459
##  Human / Radiation   8 0.016129032   0.028673835
##         Human-Kree   2 0.004032258   0.007168459
##      Human-Spartoi   1 0.002016129   0.003584229
##       Human-Vulcan   1 0.002016129   0.003584229
##    Human-Vuldarian   1 0.002016129   0.003584229
##      Icthyo Sapien   1 0.002016129   0.003584229
##            Inhuman   4 0.008064516   0.014336918
##    Kakarantharaian   1 0.002016129   0.003584229
##         Kryptonian   4 0.008064516   0.014336918
##            Martian   1 0.002016129   0.003584229
##          Metahuman   1 0.002016129   0.003584229
##             Mutant  46 0.092741935   0.164874552
##     Mutant / Clone   1 0.002016129   0.003584229
##             Planet   1 0.002016129   0.003584229
##             Saiyan   1 0.002016129   0.003584229
##           Symbiote   3 0.006048387   0.010752688
##           Talokite   1 0.002016129   0.003584229
##         Tamaranean   1 0.002016129   0.003584229
##            Ungaran   1 0.002016129   0.003584229
##            Vampire   2 0.004032258   0.007168459
##     Yoda's species   1 0.002016129   0.003584229
##      Zen-Whoberian   1 0.002016129   0.003584229
##               <NA> 217 0.437500000            NA
```

7. Among the good guys, Who are the Asgardians?

```r
good_guys %>% 
  select(race, name) %>% 
  filter(race=="Asgardian")
```

```
## # A tibble: 3 x 2
##   race      name     
##   <chr>     <chr>    
## 1 Asgardian Sif      
## 2 Asgardian Thor     
## 3 Asgardian Thor Girl
```
_side note-- whoooo is Thor Girl? Also was Loki REALLY "bad", or just put in a crappy situation?_

8. Among the bad guys, who are the male humans over 200 inches in height?


```r
#Need to remake the bad_guys dframe to include gender and height.
names(superhero_info)
```

```
##  [1] "name"       "gender"     "eye_color"  "race"       "hair_color"
##  [6] "height"     "publisher"  "skin_color" "alignment"  "weight"
```

```r
bad_guys <-
  superhero_info %>%
  select(name, alignment, race, gender, height) %>%
  filter(alignment == "bad")

bad_guys
```

```
## # A tibble: 207 x 5
##    name          alignment race              gender height
##    <chr>         <chr>     <chr>             <chr>   <dbl>
##  1 Abomination   bad       Human / Radiation Male      203
##  2 Abraxas       bad       Cosmic Entity     Male       NA
##  3 Absorbing Man bad       Human             Male      193
##  4 Air-Walker    bad       <NA>              Male      188
##  5 Ajax          bad       Cyborg            Male      193
##  6 Alex Mercer   bad       Human             Male       NA
##  7 Alien         bad       Xenomorph XX121   Male      244
##  8 Amazo         bad       Android           Male      257
##  9 Ammo          bad       Human             Male      188
## 10 Angela        bad       <NA>              Female     NA
## # … with 197 more rows
```

```r
#Then find the males over 200 inches
bad_guys %>% 
  select(name, alignment, race, gender, height) %>% 
  filter(gender == "Male", height > 200)
```

```
## # A tibble: 22 x 5
##    name           alignment race              gender height
##    <chr>          <chr>     <chr>             <chr>   <dbl>
##  1 Abomination    bad       Human / Radiation Male      203
##  2 Alien          bad       Xenomorph XX121   Male      244
##  3 Amazo          bad       Android           Male      257
##  4 Apocalypse     bad       Mutant            Male      213
##  5 Bane           bad       Human             Male      203
##  6 Darkseid       bad       New God           Male      267
##  7 Doctor Doom    bad       Human             Male      201
##  8 Doctor Doom II bad       <NA>              Male      201
##  9 Doomsday       bad       Alien             Male      244
## 10 Killer Croc    bad       Metahuman         Male      244
## # … with 12 more rows
```

9. OK, so are there more good guys or bad guys that are bald (personal interest)?

```r
#Rewrite to include hair length

bad_guys <-
  superhero_info %>%
  select(name, alignment, race, gender, height, hair_color) %>%
  filter(alignment == "bad")

#check to see what "bald" looks like in the datafram
bad_guys$hair_color
```

```
##   [1] "No Hair"          "Black"            "No Hair"         
##   [4] "White"            "Black"            NA                
##   [7] "No Hair"          NA                 "Black"           
##  [10] NA                 "No Hair"          "No Hair"         
##  [13] NA                 "Black"            "Purple"          
##  [16] "Brown"            "Black"            NA                
##  [19] NA                 "Black"            "Brown"           
##  [22] NA                 NA                 NA                
##  [25] "Black"            "Black"            "Black"           
##  [28] "No Hair"          "White"            "Black"           
##  [31] NA                 "Brown"            "Brown"           
##  [34] "Brown"            "Brown"            "No Hair"         
##  [37] "Black"            NA                 "No Hair"         
##  [40] "blond"            "Black"            "Red"             
##  [43] NA                 "Black"            "Blond"           
##  [46] "Brown"            "Brown"            "Red / Grey"      
##  [49] "Black"            NA                 "Black"           
##  [52] NA                 NA                 "Black"           
##  [55] "No Hair"          NA                 NA                
##  [58] "No Hair"          "Brown"            "No Hair"         
##  [61] NA                 "Black"            "Brown"           
##  [64] "Brown"            "Brown"            "White"           
##  [67] "No Hair"          "No Hair"          NA                
##  [70] "Auburn"           "Blond"            "Red"             
##  [73] "Black"            "Black"            "Brown"           
##  [76] "Blue"             NA                 "No Hair"         
##  [79] "Black"            "Black"            "Red"             
##  [82] "Red"              NA                 NA                
##  [85] "Black"            "White"            NA                
##  [88] "Auburn"           "Auburn"           "Blond"           
##  [91] "No Hair"          "Black"            "Grey"            
##  [94] "Brown"            "No Hair"          "Black"           
##  [97] "Green"            NA                 "Brown"           
## [100] "No Hair"          "Blond"            "No Hair"         
## [103] "No Hair"          "No Hair"          "Black"           
## [106] "Black"            NA                 "Black"           
## [109] "Black"            "No Hair"          "No Hair"         
## [112] "Red"              NA                 "No Hair"         
## [115] "Black"            NA                 "Brown"           
## [118] "White"            NA                 "White"           
## [121] "Black"            "Red"              "Black"           
## [124] "Brown"            NA                 "Brown"           
## [127] NA                 "Black"            "Blond"           
## [130] "Brownn"           NA                 "Gold"            
## [133] "Blond"            "Black"            "Black"           
## [136] "No Hair"          "Red / Orange"     "No Hair"         
## [139] "Blond"            "No Hair"          NA                
## [142] "Blond"            NA                 "Black"           
## [145] "Grey"             "Red"              "Red"             
## [148] NA                 "Strawberry Blond" "Blond"           
## [151] "Purple"           "Blond"            "Grey"            
## [154] "No Hair"          NA                 "No Hair"         
## [157] NA                 NA                 "Brown"           
## [160] "Brown"            NA                 "Blond"           
## [163] NA                 "Brown"            "Brown"           
## [166] "Red"              "Brown"            NA                
## [169] "Brown"            "Purple"           NA                
## [172] "Strawberry Blond" NA                 "White"           
## [175] NA                 "White"            "Black"           
## [178] NA                 "Black / Blue"     "No Hair"         
## [181] "No Hair"          NA                 NA                
## [184] NA                 NA                 NA                
## [187] "Brown"            "No Hair"          "No Hair"         
## [190] "White"            "Black"            NA                
## [193] NA                 "White"            "No Hair"         
## [196] "Black"            "Strawberry Blond" "Black"           
## [199] "Brown"            NA                 "No Hair"         
## [202] "Black"            "Black"            NA                
## [205] "Black"            "No Hair"          "Brown"
```

```r
#listed as "No Hair", so...

bad_guys <-
  superhero_info %>%
  select(name, alignment, race, gender, height, hair_color) %>%
  filter(hair_color == "No Hair", alignment=="bad")

#print this out
bad_guys
```

```
## # A tibble: 35 x 6
##    name          alignment race              gender height hair_color
##    <chr>         <chr>     <chr>             <chr>   <dbl> <chr>     
##  1 Abomination   bad       Human / Radiation Male    203   No Hair   
##  2 Absorbing Man bad       Human             Male    193   No Hair   
##  3 Alien         bad       Xenomorph XX121   Male    244   No Hair   
##  4 Annihilus     bad       <NA>              Male    180   No Hair   
##  5 Anti-Monitor  bad       God / Eternal     Male     61   No Hair   
##  6 Black Manta   bad       Human             Male    188   No Hair   
##  7 Bloodwraith   bad       <NA>              Male     30.5 No Hair   
##  8 Brainiac      bad       Android           Male    198   No Hair   
##  9 Darkseid      bad       New God           Male    267   No Hair   
## 10 Darth Vader   bad       Cyborg            Male    198   No Hair   
## # … with 25 more rows
```
Yep, looks like there are quite a few bad bald guys out there.


```r
#Rewrite to includ hair length

good_guys <-
  superhero_info %>%
  select(name, alignment, race, gender, height, hair_color) %>%
  filter(alignment == "good", hair_color=="No Hair")

good_guys
```

```
## # A tibble: 37 x 6
##    name            alignment race          gender height hair_color
##    <chr>           <chr>     <chr>         <chr>   <dbl> <chr>     
##  1 A-Bomb          good      Human         Male      203 No Hair   
##  2 Abe Sapien      good      Icthyo Sapien Male      191 No Hair   
##  3 Abin Sur        good      Ungaran       Male      185 No Hair   
##  4 Beta Ray Bill   good      <NA>          Male      201 No Hair   
##  5 Bishop          good      Mutant        Male      198 No Hair   
##  6 Black Lightning good      <NA>          Male      185 No Hair   
##  7 Blaquesmith     good      <NA>          <NA>       NA No Hair   
##  8 Bloodhawk       good      Mutant        Male       NA No Hair   
##  9 Crimson Dynamo  good      <NA>          Male      180 No Hair   
## 10 Donatello       good      Mutant        Male       NA No Hair   
## # … with 27 more rows
```
There are 2 more good guys with bald heads!

10. Let's explore who the really "big" superheros are. In the `superhero_info` data, which have a height over 300 or weight over 450?


```r
superhero_info %>% 
  select(name, height, weight) %>% 
  filter(height>300 | weight>450)
```

```
## # A tibble: 14 x 3
##    name          height weight
##    <chr>          <dbl>  <dbl>
##  1 Bloodaxe       218      495
##  2 Darkseid       267      817
##  3 Fin Fang Foom  975       18
##  4 Galactus       876       16
##  5 Giganta         62.5    630
##  6 Groot          701        4
##  7 Hulk           244      630
##  8 Juggernaut     287      855
##  9 MODOK          366      338
## 10 Onslaught      305      405
## 11 Red Hulk       213      630
## 12 Sasquatch      305      900
## 13 Wolfsbane      366      473
## 14 Ymir           305.      NA
```

11. Just to be clear on the `|` operator,  have a look at the superheros over 300 in height...

```r
superhero_info %>% 
  select(name,height,weight) %>% 
  filter(height>300)
```

```
## # A tibble: 8 x 3
##   name          height weight
##   <chr>          <dbl>  <dbl>
## 1 Fin Fang Foom   975      18
## 2 Galactus        876      16
## 3 Groot           701       4
## 4 MODOK           366     338
## 5 Onslaught       305     405
## 6 Sasquatch       305     900
## 7 Wolfsbane       366     473
## 8 Ymir            305.     NA
```

12. ...and the superheros over 450 in weight. Bonus question!
Why do we not have 16 rows in question #10?

_Should this be "14"?_


```r
#Here, I assume the heroes need to be BOTH >300 in ht. AND > 450 in wt (i.e., both must be true)

superhero_info %>% 
  select(name,height,weight) %>% 
  filter(height>300, weight>450)
```

```
## # A tibble: 2 x 3
##   name      height weight
##   <chr>      <dbl>  <dbl>
## 1 Sasquatch    305    900
## 2 Wolfsbane    366    473
```

We do not have 16 rows because the "|" tells R to print out superheroes that are a height over 200 *AND/OR* weight over 300. Both conditions don't have to be true, so there are more data that get pulled from the original dataframe.

## Height to Weight Ratio

13. It's easy to be strong when you are heavy and tall, but who is heavy and short? Which superheros have the lowest height to weight ratio?


```r
superhero_info %>% 
  mutate(ratio=height/weight) %>%
  select(name, ratio) %>% 
  arrange(ratio)
```

```
## # A tibble: 734 x 2
##    name         ratio
##    <chr>        <dbl>
##  1 Giganta     0.0992
##  2 Utgard-Loki 0.262 
##  3 Darkseid    0.327 
##  4 Juggernaut  0.336 
##  5 Red Hulk    0.338 
##  6 Sasquatch   0.339 
##  7 Hulk        0.387 
##  8 Bloodaxe    0.440 
##  9 Thanos      0.454 
## 10 A-Bomb      0.460 
## # … with 724 more rows
```
Giganta.

## `superhero_powers`
Have a quick look at the `superhero_powers` data frame.  


```r
glimpse(superhero_powers)
```

```
## Rows: 667
## Columns: 168
## $ hero_names                   <chr> "3-D Man", "A-Bomb", "Abe Sapien", "Abin…
## $ agility                      <lgl> TRUE, FALSE, TRUE, FALSE, FALSE, FALSE, …
## $ accelerated_healing          <lgl> FALSE, TRUE, TRUE, FALSE, TRUE, FALSE, F…
## $ lantern_power_ring           <lgl> FALSE, FALSE, FALSE, TRUE, FALSE, FALSE,…
## $ dimensional_awareness        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE,…
## $ cold_resistance              <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE,…
## $ durability                   <lgl> FALSE, TRUE, TRUE, FALSE, FALSE, FALSE, …
## $ stealth                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ energy_absorption            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ flight                       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE,…
## $ danger_sense                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ underwater_breathing         <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE,…
## $ marksmanship                 <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE,…
## $ weapons_master               <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE,…
## $ power_augmentation           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ animal_attributes            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ longevity                    <lgl> FALSE, TRUE, TRUE, FALSE, FALSE, FALSE, …
## $ intelligence                 <lgl> FALSE, FALSE, TRUE, FALSE, TRUE, TRUE, F…
## $ super_strength               <lgl> TRUE, TRUE, TRUE, FALSE, TRUE, TRUE, TRU…
## $ cryokinesis                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ telepathy                    <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE,…
## $ energy_armor                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ energy_blasts                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ duplication                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ size_changing                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE,…
## $ density_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ stamina                      <lgl> TRUE, TRUE, TRUE, FALSE, TRUE, FALSE, FA…
## $ astral_travel                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ audio_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ dexterity                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ omnitrix                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ super_speed                  <lgl> TRUE, FALSE, FALSE, FALSE, TRUE, TRUE, F…
## $ possession                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ animal_oriented_powers       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ weapon_based_powers          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ electrokinesis               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ darkforce_manipulation       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ death_touch                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ teleportation                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE,…
## $ enhanced_senses              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ telekinesis                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ energy_beams                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ magic                        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE,…
## $ hyperkinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ jump                         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ clairvoyance                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ dimensional_travel           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE,…
## $ power_sense                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ shapeshifting                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ peak_human_condition         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ immortality                  <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, TRUE, …
## $ camouflage                   <lgl> FALSE, TRUE, FALSE, FALSE, FALSE, FALSE,…
## $ element_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ phasing                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ astral_projection            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ electrical_transport         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ fire_control                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ projection                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ summoning                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ enhanced_memory              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ reflexes                     <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE,…
## $ invulnerability              <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, TRUE, …
## $ energy_constructs            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ force_fields                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ self_sustenance              <lgl> FALSE, TRUE, FALSE, FALSE, FALSE, FALSE,…
## $ anti_gravity                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ empathy                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ power_nullifier              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ radiation_control            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ psionic_powers               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ elasticity                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ substance_secretion          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ elemental_transmogrification <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ technopath_cyberpath         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ photographic_reflexes        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ seismic_power                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ animation                    <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, FALSE,…
## $ precognition                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ mind_control                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ fire_resistance              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ power_absorption             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ enhanced_hearing             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ nova_force                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ insanity                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ hypnokinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ animal_control               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ natural_armor                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ intangibility                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ enhanced_sight               <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE,…
## $ molecular_manipulation       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE,…
## $ heat_generation              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ adaptation                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ gliding                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ power_suit                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ mind_blast                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ probability_manipulation     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ gravity_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ regeneration                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ light_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ echolocation                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ levitation                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ toxin_and_disease_control    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ banish                       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ energy_manipulation          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE,…
## $ heat_resistance              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ natural_weapons              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ time_travel                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ enhanced_smell               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ illusions                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ thirstokinesis               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ hair_manipulation            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ illumination                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ omnipotent                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ cloaking                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ changing_armor               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ power_cosmic                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE,…
## $ biokinesis                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ water_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ radiation_immunity           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ vision_telescopic            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ toxin_and_disease_resistance <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ spatial_awareness            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ energy_resistance            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ telepathy_resistance         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ molecular_combustion         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ omnilingualism               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ portal_creation              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ magnetism                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ mind_control_resistance      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ plant_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ sonar                        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ sonic_scream                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ time_manipulation            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ enhanced_touch               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ magic_resistance             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ invisibility                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ sub_mariner                  <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE,…
## $ radiation_absorption         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ intuitive_aptitude           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ vision_microscopic           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ melting                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ wind_control                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ super_breath                 <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, FALSE,…
## $ wallcrawling                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ vision_night                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ vision_infrared              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ grim_reaping                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ matter_absorption            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ the_force                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ resurrection                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ terrakinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ vision_heat                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ vitakinesis                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ radar_sense                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ qwardian_power_ring          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ weather_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ vision_x_ray                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ vision_thermal               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ web_creation                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ reality_warping              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ odin_force                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ symbiote_costume             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ speed_force                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ phoenix_force                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ molecular_dissipation        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ vision_cryo                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ omnipresent                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
## $ omniscient                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE…
```

14. How many superheros have a combination of accelerated healing, durability, and super strength?

```r
tabyl(superhero_powers, accelerated_healing)
```

```
##  accelerated_healing   n   percent
##                FALSE 489 0.7331334
##                 TRUE 178 0.2668666
```

```r
tabyl(superhero_powers, durability)
```

```
##  durability   n   percent
##       FALSE 410 0.6146927
##        TRUE 257 0.3853073
```

```r
tabyl(superhero_powers, super_strength)
```

```
##  super_strength   n   percent
##           FALSE 307 0.4602699
##            TRUE 360 0.5397301
```
Of all the superheroes, 178 have accelerated healing, 257 have durability, and 360 have super strength. 

## `kinesis`

15. We are only interested in the superheros that do some kind of "kinesis". How would we isolate them from the `superhero_powers` data?


```r
kinesis<-
superhero_powers %>% 
  select(hero_names, contains("kinesis"))

kinesis
```

```
## # A tibble: 667 x 10
##    hero_names cryokinesis electrokinesis telekinesis hyperkinesis hypnokinesis
##    <chr>      <lgl>       <lgl>          <lgl>       <lgl>        <lgl>       
##  1 3-D Man    FALSE       FALSE          FALSE       FALSE        FALSE       
##  2 A-Bomb     FALSE       FALSE          FALSE       FALSE        FALSE       
##  3 Abe Sapien FALSE       FALSE          FALSE       FALSE        FALSE       
##  4 Abin Sur   FALSE       FALSE          FALSE       FALSE        FALSE       
##  5 Abominati… FALSE       FALSE          FALSE       FALSE        FALSE       
##  6 Abraxas    FALSE       FALSE          FALSE       FALSE        FALSE       
##  7 Absorbing… FALSE       FALSE          FALSE       FALSE        FALSE       
##  8 Adam Monr… FALSE       FALSE          FALSE       FALSE        FALSE       
##  9 Adam Stra… FALSE       FALSE          FALSE       FALSE        FALSE       
## 10 Agent Bob  FALSE       FALSE          FALSE       FALSE        FALSE       
## # … with 657 more rows, and 4 more variables: thirstokinesis <lgl>,
## #   biokinesis <lgl>, terrakinesis <lgl>, vitakinesis <lgl>
```

16. Pick your favorite superhero and let's see their powers!

```r
ant_man_rocks<-
superhero_powers %>% 
  filter(hero_names=="Ant-Man")

glimpse(ant_man_rocks)
```

```
## Rows: 1
## Columns: 168
## $ hero_names                   <chr> "Ant-Man"
## $ agility                      <lgl> FALSE
## $ accelerated_healing          <lgl> FALSE
## $ lantern_power_ring           <lgl> FALSE
## $ dimensional_awareness        <lgl> FALSE
## $ cold_resistance              <lgl> FALSE
## $ durability                   <lgl> FALSE
## $ stealth                      <lgl> FALSE
## $ energy_absorption            <lgl> FALSE
## $ flight                       <lgl> FALSE
## $ danger_sense                 <lgl> FALSE
## $ underwater_breathing         <lgl> FALSE
## $ marksmanship                 <lgl> FALSE
## $ weapons_master               <lgl> FALSE
## $ power_augmentation           <lgl> FALSE
## $ animal_attributes            <lgl> FALSE
## $ longevity                    <lgl> FALSE
## $ intelligence                 <lgl> TRUE
## $ super_strength               <lgl> FALSE
## $ cryokinesis                  <lgl> FALSE
## $ telepathy                    <lgl> FALSE
## $ energy_armor                 <lgl> FALSE
## $ energy_blasts                <lgl> FALSE
## $ duplication                  <lgl> FALSE
## $ size_changing                <lgl> TRUE
## $ density_control              <lgl> FALSE
## $ stamina                      <lgl> FALSE
## $ astral_travel                <lgl> FALSE
## $ audio_control                <lgl> FALSE
## $ dexterity                    <lgl> FALSE
## $ omnitrix                     <lgl> FALSE
## $ super_speed                  <lgl> FALSE
## $ possession                   <lgl> FALSE
## $ animal_oriented_powers       <lgl> FALSE
## $ weapon_based_powers          <lgl> FALSE
## $ electrokinesis               <lgl> FALSE
## $ darkforce_manipulation       <lgl> FALSE
## $ death_touch                  <lgl> FALSE
## $ teleportation                <lgl> FALSE
## $ enhanced_senses              <lgl> FALSE
## $ telekinesis                  <lgl> FALSE
## $ energy_beams                 <lgl> FALSE
## $ magic                        <lgl> FALSE
## $ hyperkinesis                 <lgl> FALSE
## $ jump                         <lgl> FALSE
## $ clairvoyance                 <lgl> FALSE
## $ dimensional_travel           <lgl> FALSE
## $ power_sense                  <lgl> FALSE
## $ shapeshifting                <lgl> FALSE
## $ peak_human_condition         <lgl> FALSE
## $ immortality                  <lgl> FALSE
## $ camouflage                   <lgl> FALSE
## $ element_control              <lgl> FALSE
## $ phasing                      <lgl> FALSE
## $ astral_projection            <lgl> FALSE
## $ electrical_transport         <lgl> FALSE
## $ fire_control                 <lgl> FALSE
## $ projection                   <lgl> FALSE
## $ summoning                    <lgl> FALSE
## $ enhanced_memory              <lgl> FALSE
## $ reflexes                     <lgl> FALSE
## $ invulnerability              <lgl> FALSE
## $ energy_constructs            <lgl> FALSE
## $ force_fields                 <lgl> FALSE
## $ self_sustenance              <lgl> FALSE
## $ anti_gravity                 <lgl> FALSE
## $ empathy                      <lgl> FALSE
## $ power_nullifier              <lgl> FALSE
## $ radiation_control            <lgl> FALSE
## $ psionic_powers               <lgl> FALSE
## $ elasticity                   <lgl> FALSE
## $ substance_secretion          <lgl> FALSE
## $ elemental_transmogrification <lgl> FALSE
## $ technopath_cyberpath         <lgl> FALSE
## $ photographic_reflexes        <lgl> FALSE
## $ seismic_power                <lgl> FALSE
## $ animation                    <lgl> FALSE
## $ precognition                 <lgl> FALSE
## $ mind_control                 <lgl> FALSE
## $ fire_resistance              <lgl> FALSE
## $ power_absorption             <lgl> FALSE
## $ enhanced_hearing             <lgl> FALSE
## $ nova_force                   <lgl> FALSE
## $ insanity                     <lgl> FALSE
## $ hypnokinesis                 <lgl> FALSE
## $ animal_control               <lgl> FALSE
## $ natural_armor                <lgl> FALSE
## $ intangibility                <lgl> FALSE
## $ enhanced_sight               <lgl> FALSE
## $ molecular_manipulation       <lgl> FALSE
## $ heat_generation              <lgl> FALSE
## $ adaptation                   <lgl> FALSE
## $ gliding                      <lgl> FALSE
## $ power_suit                   <lgl> FALSE
## $ mind_blast                   <lgl> FALSE
## $ probability_manipulation     <lgl> FALSE
## $ gravity_control              <lgl> FALSE
## $ regeneration                 <lgl> FALSE
## $ light_control                <lgl> FALSE
## $ echolocation                 <lgl> FALSE
## $ levitation                   <lgl> FALSE
## $ toxin_and_disease_control    <lgl> FALSE
## $ banish                       <lgl> FALSE
## $ energy_manipulation          <lgl> FALSE
## $ heat_resistance              <lgl> FALSE
## $ natural_weapons              <lgl> FALSE
## $ time_travel                  <lgl> FALSE
## $ enhanced_smell               <lgl> FALSE
## $ illusions                    <lgl> FALSE
## $ thirstokinesis               <lgl> FALSE
## $ hair_manipulation            <lgl> FALSE
## $ illumination                 <lgl> FALSE
## $ omnipotent                   <lgl> FALSE
## $ cloaking                     <lgl> FALSE
## $ changing_armor               <lgl> FALSE
## $ power_cosmic                 <lgl> FALSE
## $ biokinesis                   <lgl> FALSE
## $ water_control                <lgl> FALSE
## $ radiation_immunity           <lgl> FALSE
## $ vision_telescopic            <lgl> FALSE
## $ toxin_and_disease_resistance <lgl> FALSE
## $ spatial_awareness            <lgl> FALSE
## $ energy_resistance            <lgl> FALSE
## $ telepathy_resistance         <lgl> FALSE
## $ molecular_combustion         <lgl> FALSE
## $ omnilingualism               <lgl> FALSE
## $ portal_creation              <lgl> FALSE
## $ magnetism                    <lgl> FALSE
## $ mind_control_resistance      <lgl> FALSE
## $ plant_control                <lgl> FALSE
## $ sonar                        <lgl> FALSE
## $ sonic_scream                 <lgl> FALSE
## $ time_manipulation            <lgl> FALSE
## $ enhanced_touch               <lgl> FALSE
## $ magic_resistance             <lgl> FALSE
## $ invisibility                 <lgl> FALSE
## $ sub_mariner                  <lgl> FALSE
## $ radiation_absorption         <lgl> FALSE
## $ intuitive_aptitude           <lgl> FALSE
## $ vision_microscopic           <lgl> FALSE
## $ melting                      <lgl> FALSE
## $ wind_control                 <lgl> FALSE
## $ super_breath                 <lgl> FALSE
## $ wallcrawling                 <lgl> FALSE
## $ vision_night                 <lgl> FALSE
## $ vision_infrared              <lgl> FALSE
## $ grim_reaping                 <lgl> FALSE
## $ matter_absorption            <lgl> FALSE
## $ the_force                    <lgl> FALSE
## $ resurrection                 <lgl> FALSE
## $ terrakinesis                 <lgl> FALSE
## $ vision_heat                  <lgl> FALSE
## $ vitakinesis                  <lgl> FALSE
## $ radar_sense                  <lgl> FALSE
## $ qwardian_power_ring          <lgl> FALSE
## $ weather_control              <lgl> FALSE
## $ vision_x_ray                 <lgl> FALSE
## $ vision_thermal               <lgl> FALSE
## $ web_creation                 <lgl> FALSE
## $ reality_warping              <lgl> FALSE
## $ odin_force                   <lgl> FALSE
## $ symbiote_costume             <lgl> FALSE
## $ speed_force                  <lgl> FALSE
## $ phoenix_force                <lgl> FALSE
## $ molecular_dissipation        <lgl> FALSE
## $ vision_cryo                  <lgl> FALSE
## $ omnipresent                  <lgl> FALSE
## $ omniscient                   <lgl> FALSE
```

Ant-Man is a good-guy from the Marvel Universe that shrinks down to the size of an ant using a fancy suit. His powers, listed above, are "intelligence" and "size-changing". He's my favorite because Paul Rudd plays him in the movies, and Paul Rudd is both adorable & hilarious.

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
