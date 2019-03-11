---
layout: post
title: "A talk on Java's HashSets and TreeSets!"
excerpt: "Find my slides here."
categories: [development, speaking]
image:
    feature: 
comments: true
---

So recently I took the chance to give a talk on Java's HashSets and TreeSets, demonstrating sample code along with the key features of both data structures.

Find my sample code and my slides here.

<iframe src="https://docs.google.com/presentation/d/e/2PACX-1vSeppAxZUE2nbTzgzhPBAHQchv59z2SPABWIu-64BVyMo4WQsdJi6yV02yzpvyamhuitPqEQGTqaWU1/embed?start=false&loop=false&delayms=3000" frameborder="0" width="960" height="569" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>

And here's my sample source code:

```java

package sets;
import java.util.*;

public class Sets {
    
    public static void display(HashSet<String> x){
        for(String i: x)System.out.println(i);
    }
    
    public static void displaytree(TreeSet<String> x){
        for(String i:x)System.out.println(i);
    }
    
    public static void main(String[] args) {        
        HashSet<String> hs = new HashSet<String>();        
        
        hs.clear(); // clears data in the HashSet        
        
        // adding elements to the HashSet 
        hs.add("Aditya"); hs.add("Ramesh"); hs.add("Aditya"); hs.add("Adi"); hs.add("Z");
        
        // displaying the HashSet
        display(hs);
        
        // removing an element from the HashSet
        hs.remove("Ramesh");        
        
        // checking if "Aditya" is in the HashSet
        if(hs.contains("Aditya")) System.out.println("Aditya is present in the HashSet");
        else System.out.println("Not found in HashSet");
        
        HashSet<String> cpy = new HashSet<String>();
        cpy = (HashSet)hs.clone(); // cloning the HashSet
        
        /* -------------------------------------------------- */
        
        
        TreeSet<String> ts = new TreeSet<String>();
        
        ts.clear(); // clears data in the TreeSet        
        
        ts.add("Aditya"); ts.add("Ramesh"); ts.add("Z"); ts.add("Aditya"); ts.add("Adi"); 
        
        // displaying the TreeSet
        displaytree(ts);
        
        // removing an element from the TreeSet
        ts.remove("Ramesh");        
        
        // checking if "Aditya" is in TreeSet
        if(ts.contains("Aditya")) System.out.println("Aditya is present in the TreeSet");
        else System.out.println("Not found in TreeSet");
        
        TreeSet<String> treecpy = new TreeSet<String>(); 
        treecpy = (TreeSet)ts.clone(); // copies the TreeSet
       
    }
}



```