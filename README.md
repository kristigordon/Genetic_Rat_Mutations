![image](https://user-images.githubusercontent.com/66803124/119214591-dd831b00-ba7c-11eb-84f6-799b89c4dff9.png)

# Natural Selection or Massive Mutation?

  Certain problems are a little too elegant for any brute force solution and the creation of new life is just one of those things. Genetic algorithms are used to partially 
mimic the process of natural selection and belong under the umbrella of evolution algorithms. Genetic algorithms are commonly in optimiztion and search problems due to their ability to generate high-quality solutions. By relying on biologically inspired operators such as mutation, crossover and selection, natural selection can be simiulated 
and used to predict potential results without the need for the physical harm of, in this case alone, thousands of animals.

  Today, we will create a massive strain of rats using Python and genetic algorithms. Unlike exhaustive search engines, which use pure brute force, genetic algorithms don’t
try every possible solution. Instead, they continuously grade solutions and then use them to make “informed guesses” going forward. In our case, this will mean we are uliltilizing Python's random and statistics libraries to attempt to mimic the way this biological selection of traits would occur. For our experiment, weight is the 
main indicator of a solution's viability. The largest rats, should over time, create even larger rats if paired together with some of the largest rats in the opposite sex's population.  

PHEW. Science! 
Enough talk. Let's kick it all off by naming some variables.
```
GOAL = 50000
NUM_RATS = 20
INITIAL_MIN_WT = 200
INITIAL_MAX_WT = 600
INITIAL_MODE_WT = 300
MUTATE_ODDS = 0.01
MUTATE_MIN = 0.5
MUTATE_MAX = 1.2
LITTER_SIZE = 8
LITTERS_PER_YEAR = 10
GENERATION_LIMIT = 500
```

Ensure even number of rats for breeding pairs by using the modulo function. This function will divide the numerical value by the number chosen, in this case we need an even number of rats so % 2 will work great. If a number is 10 % 2, this will be 0 remaining since 2 can go into 10 5 times. However, 11 % 2 would be 1 since there would be 1 remaining after dividing the two numbers.
```
if NUM_RATS % 2 != 0:
    NUM_RATS += 1
import time
import random
import statistics 
rat = random.triangular(INITIAL_MIN_WT, INITIAL_MAX_WT, INITIAL_MODE_WT) 
```
STEP 1: Randomly generate a population of solutions.
```
print(rat)
519.8376953468256

rats = []

for i in range(20):
    random_weight = random.triangular(INITIAL_MIN_WT, INITIAL_MAX_WT, INITIAL_MODE_WT)
    rats.append(int(random_weight))
```
To see how this is looking so far:
```
print(rats)
[319, 484, 364, 310, 299, 244, 439, 320, 260, 315, 524, 304, 224, 340, 293, 313, 439, 424, 465, 318]
```
STEP 2: Measure the fitness of the population - 

By taking the average weight and comparing it with our goal. if avg wt is >= goal, we can stop. 
```
average = statistics.mean(rats)
marker = average/GOAL
```

```
print(average)
print(marker)
349.9
0.006998
```
STEP 3: Select the best solution and discard the rest
Num Rats = 20, we want to keep the population at 20. 

If population is > 20, we need to take out extra rats and keep the best ones. 

rats.append(200)
rats.append(600)
^ Used to add those values but every time you run, appends the values again

![image](https://user-images.githubusercontent.com/66803124/119214357-0d312380-ba7b-11eb-815c-c03032aa14ac.png)

```
to_retain = 20
sorted_rats = sorted(rats)
to_retain_by_sex = to_retain // 2
```
Since we are retaining 20, we want to // 2 to have 10 of each sex.
// double slash is in case there is an odd number, we still get an even answer.
```
members_per_sex = len(rats) // 2
females = sorted_rats[:11]
males = sorted_rats[11:]
females_to_keep = females[-10:]
males_to_keep = males[-10:]
```
-10 in these sorted lists means that we are keep the 10 heaviest rats.
```
print(sorted_rats)
[200, 224, 244, 260, 293, 299, 304, 310, 313, 315, 318, 319, 320, 340, 364, 424, 439, 439, 465, 484, 524, 600]
print(females)
[200, 224, 244, 260, 293, 299, 304, 310, 313, 315, 318]
print(males)
[319, 320, 340, 364, 424, 439, 439, 465, 484, 524, 600]
print(females_to_keep)
[224, 244, 260, 293, 299, 304, 310, 313, 315, 318]
print(males_to_keep)
[320, 340, 364, 424, 439, 439, 465, 484, 524, 600]
```
STEP 4: Cross over elements in the best solutions to make new solutions

So take the selected females and males that we kept, the heaviest pairs, and use them to
cross over (breed) and make new solutions (children).

![image](https://user-images.githubusercontent.com/66803124/119214465-f0e1b680-ba7b-11eb-8c14-d1e3d67d1482.png)

```
random.shuffle(females_to_keep)
random.shuffle(males_to_keep)
print(females_to_keep)
print(males_to_keep)

children = []
litter_size = 8
pairs = zip(females_to_keep, males_to_keep)
```
Zip takes each item from each list and puts them together, in this case it pairs a female
from our list of kept female rats with a male from our list of kept male rats.

This creates a tuple, which is similar to a list but is instead immutable or 
unchangable once created.
```
for female, male in zip(females_to_keep, males_to_keep):
    for child in range(litter_size):
        child = random.randint(female, male)
```
    # randint allows for a random integer to be created that is between the weight of the 
    # female and male parents, thus creating their child.
    # Since we have our females lighter than males, we can go (females, males) since they ascending
        children.append(child)
    # Now we append the child into the list of children
```
print(list(pairs))
print(children)
[318, 299, 315, 224, 293, 304, 313, 310, 244, 260]
[320, 424, 340, 439, 600, 465, 439, 484, 524, 364]
[(318, 320), (299, 424), (315, 340), (224, 439), (293, 600), (304, 465), (313, 439), (310, 484), (244, 524), (260, 364)]
[318, 319, 320, 320, 318, 319, 320, 320, 385, 413, 349, 312, 302, 302, 309, 323, 337, 319, 316, 319, 328, 333, 340, 338, 356, 314, 417, 291, 361, 238, 318, 367, 382, 385, 494, 395, 512, 367, 384, 414, 456, 431, 306, 383, 342, 375, 352, 397, 357, 359, 331, 322, 350, 421, 421, 356, 337, 448, 384, 418, 311, 449, 336, 409, 286, 440, 395, 264, 460, 470, 379, 354, 339, 273, 350, 294, 288, 280, 324, 336]
```
STEP 5: Mutate a small number of elements in the solutions by changing their value.

The number of elements in the list of children.

Enumerate allows you to keep count of the list and change the elements of the list. 
```
for index, rat in enumerate(children):
    print(random.random())   
    if MUTATE_ODDS >= random.random():
    children[index] = round(rat * random.uniform(MUTATE_MIN, MUTATE_MAX))
    
```
MUTATE_ODDS=0.01 Probability of a mutation occurring in a rat

If the mutate odds are >= the random.random, we are going to mutate the child.
 
round is to round the number back to a whole number after multiplying it by the random.uniform
random.uniform is going to give a random floating point number between the mutate min
and mutate max in our case. 

MUTATE_MIN=0.5 Scalar on rat weight of least beneficial mutation
MUTATE_MAX=1.2 Scalar on rat weight of most beneficial mutation
```
print(children)
    
0.29172007862188754
0.23074903365345023
0.6592079873055271
0.583289912784931
0.5959619087024186
0.1901568418324462
0.44567805175261166
0.06565815106399986
0.4296197123110166
0.27742780284566304
0.4978902398772106
0.9201496543934607
0.7075208243077078
0.7654079362211721
0.43800310510192975
0.029625865353035685
0.6029724134755202
0.5767836485375342
0.3178336069703469
0.7976706814763387
0.4927084148841454
0.8137024730648956
0.6512481847982352
0.35950722197212737
0.6773327755678445
0.7646260938112954
0.6959915822150122
0.5423316018042611
0.7843665435289418
0.5235720254136057
0.7343381551202728
0.7303815378464624
0.6569240441122978
0.425870500245078
0.5772307774347156
0.5905721182729177
0.934098306586117
0.16646055550423478
0.9335096291267659
0.4890916848680148
0.7932310028441745
0.9179462578368046
0.09111740967633897
0.8295734606076679
0.5120040040459357
0.5234013347980573
0.8036527795472461
0.9413745377018661
0.48250831885405243
0.5329124890269311
0.17955163188643075
0.1878489645911381
0.7327942029026692
0.8280575284271964
0.7515525451118185
0.08804357852172418
0.29310022825904936
0.774684797289349
0.9367053845938458
0.24973985209824423
0.7326553605632323
0.5431467042299041
0.10484624158815381
0.89987976630996
0.28364411862066663
0.2178479414250748
0.2771615347142128
0.7135795800775341
0.5238365892778297
0.7873100584838316
0.2700612676174343
0.3102844624473673
0.07701253339699432
0.07363046801666229
0.4495512875689227
0.8235641380964822
0.09201942002522434
0.7936066710123235
0.8345900197883804
0.5078689825902895
[318, 319, 320, 320, 318, 319, 320, 320, 385, 413, 349, 312, 302, 302, 309, 323, 337, 319, 316, 319, 328, 333, 340, 338, 356, 314, 417, 291, 361, 238, 318, 367, 382, 385, 494, 395, 512, 367, 384, 414, 456, 431, 306, 383, 342, 375, 352, 397, 357, 359, 331, 322, 350, 421, 421, 356, 337, 448, 384, 418, 311, 449, 336, 409, 286, 440, 395, 264, 460, 470, 379, 354, 339, 273, 350, 294, 288, 280, 324, 336]
```
```
def populate (num_rats, min_wt, max_wt, mode_wt):
    return [int(random.triangular(min_wt, max_wt, mode_wt)) for i in range(num_rats)]

def fitness(population, goal):
```
Measure population fitness based on an attribute mean vs target.
```
    ave = statistics.mean(population)
    return ave / goal

def select(population, to_retain):
```
Population is the list, so in our example "rats"
```
sorted_population = sorted(population)
```
sort the population ascending
```
to_retain_by_sex = to_retain//2
```
out of the 20 we are retaining, we want 10 in each sex
```
members_per_sex = len(sorted_population)//2
```
the sorted population may have more than 20, so we take that length and put half in each sex
```
females = sorted_population[:members_per_sex]
```
take the sorted pop and start at the beginning and take the first half for females
```
males = sorted_population[members_per_sex:]
```
take the sorted pop and start at the halfway point and take the second half for males
```
selected_females = females[-to_retain_by_sex:]
```
since there could more than 20 total rats, we need to ensure the females only have 10.
```
-to_retain_by_sex means taking the last 10 out of the sorted list of females,
so the last 10 in the list would be the 10 heaviest. (Same for males)
```
```
selected_males = males[-to_retain_by_sex:]
```
and that the males only have 10
```
return selected_males, selected_females
    
def breed(males, females, litter_size):
```
Crossover genes among members (weights) of a population.
```
random.shuffle(males)
random.shuffle(females)
```
Shuffle the values
```
children = []
```
Create a list
```
for male, female in zip(males, females):
```
Creating tuples for each pair
```
for child in range(litter_size):
```
Litter size 8
```
child = random.randint(female, male)
```
breed a child between the female and male weights
```
children.append(child)
```
add that child to the list of children
```
    return children
```
```
def mutate(children, mutate_odds, mutate_min, mutate_max):
```
Randomly alter rat weights using input odds & fractional changes.
```
for index, rat in enumerate(children):
if mutate_odds >= random.random():
children[index] = round(rat * random.uniform(mutate_min, mutate_max))
return children
```

Thank you for making it this far! I hope you found some interesting ideas and got to see another level of coding beyond the more traditional uses. 
Code is so dynamic and has a limitless amount of applications. Keep creating. Keep learning. 

![image](https://user-images.githubusercontent.com/66803124/119214487-1ec6fb00-ba7c-11eb-8af4-e704e2143812.png)
