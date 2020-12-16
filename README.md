# NeuralNames
A random person name generator backed by a neural network.  The model is fully trained with 3000 popular American names (1000 surnames, 1000 boy names, and 1000 girl names) in order to predict character sequence. Names are generated based on the spelling patterns of the sample data. Light-weight. No dependencies. 

### This library uses a stand-alone, proprietary, light-weight neural network and does not use any commercial or open source machine learning frameworks or components.

### Neural Network
* 78 input nodes
* 50 hidden nodes (1 layer)
* 27 output nodes

#### Input nodes (Character embeddings)
Each node represents a character. The previous 3 characters (78 nodes) are given to the network to predict next characters (output).
For the very first characters, all nodes are zeros. 

#### Output nodes
Each node represents a character and an end marker character. (26 + 1 nodes)
Each node has a probability (weight) of the next character. (weights)
One of the characters (nodes) will be randomly selected.
The higher the probability is for the character, the more likely it is to be selected.

#### Prediction Process
1st Character: (no-char) (no-char) (no-char) -> predict 1st char
2nd Character: (no-char) (no-char) (1st char) -> predict next char
3rd Character: (no-char) (1st-char) (2nd char) -> predict next char (including the end marker char)
4th Character: (1st-char) (2nd char) (3rd char) -> predict next char (including the end marker char)
:

* During the testing process, the accuracy of predicting the correct next probable character is over 95%.
* At any point, if the predicted char is the begin/end marker character, the process ends and the word is returned.

#### Final Checks
* The name that contains no vowel letter (aeiou) will be discarded; a new name will be generated. (ex. Zkct, Adn)
* The name that contains more than 2 repeated letters will also be discarded. (ex. Nikkken, Mammm)
* Each name type has a fixed range based on the sample data (Female: 3-10 chars, Male: 2-11 chars, Surname: 2-11 chars)  
* A regeneration rate is approximately 3%.


### NeuralNames.GetRandomFullName()
An easy-to-use, static method, which generates random names.
``` csharp
for (int i = 0; i < 3; i++)
{
    var name = ShereSoft.NeuralNames.GetRandomFullName();
    Console.WriteLine(name);  // Ramon Wood, Estelle Mcdan, Alen Gillowell (different names)
}
```

### new NeuralNames(Random).NextFullName()
You can create an instance with a seed, which produces the same set of names, guaraneteed.
``` csharp
var random = new Random(-5);  // Seeding provides the same values
var nameGenerator = new NeuralNames(random);
for (int i = 0; i < 3; i++)
{
    var name = nameGenerator.NextFullName();
    Console.WriteLine(name);  // Cames Weath, Hay Reed, Charlee Eclaird (guaranteed)
}
```
