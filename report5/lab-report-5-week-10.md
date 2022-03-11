<h1 align = "center">
Week 10 Lab Report: Different Bugs
</h1>

The group's [repository](https://github.com/henryzhang03/my-markdown-parse)

The main [repository](https://github.com/ucsd-cse15l-w22/markdown-parse)

## *Part 1: Finding Tests with Different Results*
With the sheer number of files, searching through them one by one would not be feasible, or at the very least very time-consuming. So, we therefore use a bash script that has the following contents:

```sh
for file in test-files/*.md;
do
    echo $file
    java MarkdownParse $file
done
```

This bash script will not only return the outputs of running each `.md` file with the given implementation, it will also indicate which file the results map to, with the line `echo $file`. Additionally, we use the command

```
bash scirpt.sh > results.txt
```

We do this for both my group's implementation and also the main implementation, storing them into their respective `results.txt` files. Next, we use the command `diff`, or more specifically:

 ```
diff my-markdown-parse/results.txt markdown-parse/results.txt
 ```

The output of the command is the following:

```
47a48
> []
90a92
> [/foo]
209a212
> [url]
226a230
> [baz]
266c270
< [/bar\* "ti\*tle"]
---
> []
488c492
< [/f&ouml;&ouml; "f&ouml;&ouml;"]
---
> []
688c692
< [url &quot;tit&quot;]
---
> []
846c850
< [/uri "title"]
---
> []
851a856
> []
854a860
> []
856c862
< [/my uri]
---
> []
858c864
< [</my uri>]
---
> []
860,861c866
< [foo
< bar]
---
> []
865,866c870
< [<foo
< bar>]
---
> []
874c878
< [\(foo\]
---
> [\(foo\)]
876c880
< [foo(and(bar]
---
> [foo(and(bar))]
878c882
< [foo(and(bar]
---
> []
880c884
< [foo\(and\(bar\]
---
> []
882c886
< [<foo(and(bar]
---
> []
898c902
< [/url "title", /url 'title', /url (title]
---
> []
900c904
< [/url "title \"&quot;"]
---
> []
904c908
< [/url "title "and" title"]
---
> []
906c910
< [/url 'title "and" title']
---
> []
908,909c912
< [   /uri
<   "title"  ]
---
> []
913c916
< []
---
> [/uri]
915c918
< []
---
> [/uri]
917c920
< []
---
> [/uri]
931c934
< []
---
> [uri1]
956a960
> [moon.jpg]
957a962
> [/uri]
1032a1038
> []
1033a1040
> []
1047c1054
< []
---
> [/url]
1049c1056
< []
---
> [/url]
1055c1062
< []
---
> [train.jpg]
1059c1066
< []
---
> [<url>]
1063c1070
< []
---
> [/url]
```
I ran into some problems here, as some tests for my group's implementation caused an infinite loop, so I had to find specific lines that would actually match up. However, doing so was not that hard, as you just find the corresponding `.md` file.

## *Part 2: Bug 1*

One test file that produced different results for my group's implementation versus the main implementation is `577.md`. Our implementation produced `[]` as the ouput, but the main implementation produced `[train.jpg]`. Using VSCode preview, we can see that there should not be a link in test file `577.md`, so the main implementation is wrong. The expected output is `[]`. One fix that is possible for the main implementation is to add the code

```java
if(nextOpenBracket > 0 && markdown.charAt(nextOpenBracket - 1) == '!') {
    flag = true;
}
```

The above code snippet checks for the exclamation mark which denotes an image.

## *Part 3: Bug 2*

One test file that produced different results for my group's implementation versus the main implementation is `32.md`. The main implementation produced `[]` as the ouput, but our implementation produced `[/f&ouml;&ouml; "f&ouml;&ouml;"]`. Using VSCode preview, we can see that there is indeed a link in test file `32.md`, and the main implementation is wrong and did not catch it. The expected output is a weird combination of `[föö]` and the reference to `/f%C3%B6%C3%B6`. Both our implementation and the main implementation are wrong because we have no way of being able to interpret weird characters. However, the main implementation was "more" wrong, in the fact that it also broke at the space and didn't include anything at all. There is no code that should be "fixed", just code that should be added to handle those 2 cases. The code to fix the space will probably require an if-statement catching spaces, and checking the conditions of the stuff around the space. The code to fix the weird characters is a bit of range, but it could be a parser that checks for combinations of characters that should produce certain other characters using some sort of library or predefined rule for each combination.