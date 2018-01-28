## Hello guys, here i want to explain some things about so called "rules extraction" from sclearn decision trees
#### We will try to extract structure of our classification tree in a form in which a person will understand

![](https://www.simafore.com/hs-fs/hub/64283/file-15380323-png/images/rapidminer-decision-tree-personal-loan-accept.png?t=1496839460448)

#### Just give me the the code!

```Python
def tree_to_code(tree, tree_index, feature_names=range(0, 28)):
    nspaces = 4
    tree_ = tree.tree_
    feature_name = [
        feature_names[i] if i != _tree.TREE_UNDEFINED else "undefined!"
        for i in tree_.feature
    ]
    
    print("int ClassificationTree" + str(tree_index) + "(const std::vector<double> &features, double& probability)")
    print("{")

    def recurse(node, depth):
        indent = " " * nspaces * depth
        
        if tree_.feature[node] != _tree.TREE_UNDEFINED:
            name = feature_name[node]
            threshold = tree_.threshold[node]
            
            print("\n{}if (features[{}] <= {:0.9f})".format(indent, name, threshold))
            print("{}".format(indent) + "{")
            
            recurse(tree_.children_left[node], depth + 1)
            
            print("{}".format(indent) + "}")
            print("{}else".format(indent) + "\n" + "{}".format(indent) + "{")
            
            recurse(tree_.children_right[node], depth + 1)
            
            print("\n" + "{}".format(indent) + "}")
        else:
            
            """
            Normalize the value of the zero index,
            which contains the number of objects of class 0 (DRONE) to the total number
            of objects that fall into current node.
            
            Example:
                tree_.value[node] returns [[21. 15. 3.]]
                It means that there are 21. object of class 0
                                        15. objects of class 1
                                        3. object of class 2
                                        21. / (21. + 15. + 3) = probability of class 0 in current node
            """
            
            print("\n{}return {:0.9f};".format(indent, tree_.value[node][0][0] / sum(tree_.value[node][0])))

    recurse(0, 1)
    
    print("}")
```

#### Here is the result:

```C++
int ClassificationTree1(const std::vector<double> &features, double& probability)
{

    if (features[24] <= 0.408988237)
    {

        if (features[17] <= 0.087502830)
        {

            return 0.000000000;
        }
        else
        {

            return 0.285714286;

        }
    }
    else
    {

        if (features[13] <= 0.396394730)
        {

            return 0.590909091;
        }
        else
        {

            return 0.189189189;

        }

    }
}
```
