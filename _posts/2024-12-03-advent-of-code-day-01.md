---
title: "Advent of Code - Day 01"
date: 2024-12-03
categories:
- Advent of Code
- .NET
- C#
- GitHub Copilot
---

Advent of Code is an Advent calendar of small programming puzzles for a variety of skill levels that can be solved in any programming language you like. People use them as interview prep, company training, university coursework, practice problems, a speed contest, or to challenge each other.

You don't need a computer science background to participate - just a little programming knowledge and some problem solving skills will get you pretty far. Nor do you need a fancy computer; every problem has a solution that completes in at most 15 seconds on ten-year-old hardware.

Advent of Code: [https://adventofcode.com/](https://adventofcode.com/)

For my take on the AoC, I'll be utilising GitHub Copilot within my IDE (Visual Studio Code), in order to determine for myself if AI code completion can assist in helping me solve the problems.

I'll not be using it to fully code the entire solution for me; like giving it the problem statement and input and Copilot spitting out code, but instead crafting some comments to aid copilot at times to suggest snippets of code that I think is boilerplate code or reviewing the code completing suggestions.

Day 01: [https://adventofcode.com/2024/day/1](https://adventofcode.com/2024/day/1)

## My Code

Code is developed using .NET/C#

```cs
    public object PartOne(string input)
    {
        var (leftList, rightList) = ParseInput(input);

        int sum = 0;
        for(int i = 0; i < leftList.Count; i++)
            sum += Math.Abs(rightList[i] - leftList[i]);

        return sum;
    }

    public object PartTwo(string input) 
    {
        var (leftList, rightList) = ParseInput(input); 

        int sum = 0;
        for(var i = 0; i < leftList.Count; i++)
            sum += Math.Abs(leftList[i] * rightList.Count(x => x == leftList[i]));

        return sum;
    }

    private (List<int> left, List<int> right) ParseInput(string input)
    {
        var lines = input.Split("\n", StringSplitOptions.RemoveEmptyEntries);
        var leftList = new List<int>();
        var rightList = new List<int>();

        foreach(var line in lines)
        {
            var parts = line.Split(" ", StringSplitOptions.RemoveEmptyEntries);
            leftList.Add(int.Parse(parts[0]));
            rightList.Add(int.Parse(parts[1]));
        }

        leftList.Sort();
        rightList.Sort();

        return (leftList, rightList);
    }
```

## What Copilot helped me with

Code completions for the following was helpful

```cs
# auto-generation of for-loops
for(var i = 0; i < leftList.Count; i++)

# Copilot knew I wanted to get all the lines of the input so suggested I split on the new line character
input.Split("\n", StringSplitOptions.RemoveEmptyEntries);
line.Split(" ", StringSplitOptions.RemoveEmptyEntries);

# with the context comment of "# sort the lists"
leftList.Sort();
rightList.Sort();

```