<h1 align = "center">
Week 8 Lab Report: Code Review
</h1>

The repository that [repository](https://github.com/henryzhang03/markdown-parse)

The other group's [repository](https://github.com/nakulnandhakumar/markdown-parse)

## *Part 1: Snippet 1*

Using VSCode preview, I saw that for snippet 1, there should be these 3 links: `` `google.com``, `google.com`, `ucsd.edu`. Below is the code snippet for the test that I ran for `snippet1.md`.

```java
@Test
public void testSnippet1() throws IOException {
    Path fileName = Path.of("snippet1.md");
    String contents = Files.readString(fileName);
    assertEquals(List.of("`google.com", "google.com", "ucsd.edu"), MarkdownParse.getLinks(contents));
}
```
Using this test for my group's repo, I get the following error:

```java
java.lang.AssertionError: expected:<[`google.com, google.com, ucsd.edu]> but was:<[url.com, `google.com, google.com]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnippet1(MarkdownParseTest.java:54)
```
Using the same test for the other group's repo, I get this: 
```java
java.lang.AssertionError: expected:<[`google.com, google.com, ucsd.edu]> but was:<[url.com, `google.com, google.com]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnippet1(MarkdownParseTest.java:50)
```

I do not think there is a small(<10 lines code) code change that will make my program work for snippet 1 and all related cases that use inline code with backticks. This would require a more involved change, as my program does not deal with backticks at all, so checking for them, I might encounter other rules about backticks in VSCode that I did not know about.

## *Part 2: Snippet 2*

For snippet 2, the expected output based on VSCode preview should be these 3 links: `a.com`, `a.com(())`, `example.com`.

For snippet 3, the test that I wrote is the following:

```java
@Test
public void testSnippet2() throws IOException {
        Path fileName = Path.of("snippet2.md");
        String contents = Files.readString(fileName);
        assertEquals(List.of("a.com", "a.com(())", "example.com"), MarkdownParse.getLinks(contents));
}
```
Using this test for my group's repo, I get the following error:

```java
java.lang.AssertionError: expected:<[a.com, a.com(()), example.com]> but was:<[a.com, a.com(()]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnippet2(MarkdownParseTest.java:61)
```
Using the same test for the other group's repo, I get this: 
```java
java.lang.StringIndexOutOfBoundsException: String index out of range: -1
        at java.base/java.lang.StringLatin1.charAt(StringLatin1.java:48)
        at java.base/java.lang.String.charAt(String.java:1512)
        at MarkdownParse.getLinks(MarkdownParse.java:25)
        at MarkdownParseTest.testSnippet2(MarkdownParseTest.java:57)
```

I do not think there is a small(<10 lines) code change that will make my
program work for snippet 2 and all related cases that nest parentheses,
brackets, and escaped brackets. This is because I need to modify my code to check for 
nested parentheses and brackets first, which I think will cause errors with other break files I created.
Then I would have to check for escaped brackets, which I think might again cause errors with fixes to my 
program that target other break files. This has made me realize that working on a project requires many fixes to
take into account different issues that rise up, and that your solution should try to be general, instead of only trying to fix one specific case.

## *Part 3: Snippet 3*

For snippet 3, the expected output based on VSCode preview should be these 3 links: `https://www.twitter.com`, `https://ucsd-cse15l-w22.github.io/`, `https://cse.ucsd.edu/`.

For snippet 2, the test that I wrote is the following:

```java
@Test
public void testSnippet3() throws IOException {
    Path fileName = Path.of("snippet3.md");
    String contents = Files.readString(fileName);
    assertEquals(List.of("https://www.twitter.com", "https://ucsd-cse15l-w22.github.io/", "https://cse.ucsd.edu/"), MarkdownParse.getLinks(contents));
}
```
Using this test for my group's repo, I get the following error:

```java
java.lang.AssertionError: expected:<[https://www.twitter.com, https://ucsd-cse15l-w22.github.io/, https://cse.ucsd.edu/]> but was:<[
    https://www.twitter.com
, 
    https://ucsd-cse15l-w22.github.io/
, github.com

And there's still some more text after that.

[this link doesn't have a closing parenthesis for a while](https://cse.ucsd.edu/



]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnippet3(MarkdownParseTest.java:68)
```
Using the same test for the other group's repo, I get this: 
```java
java.lang.StringIndexOutOfBoundsException: String index out of range: -1
        at java.base/java.lang.StringLatin1.charAt(StringLatin1.java:48)
        at java.base/java.lang.String.charAt(String.java:1512)
        at MarkdownParse.getLinks(MarkdownParse.java:25)
        at MarkdownParseTest.testSnippet3(MarkdownParseTest.java:64)
```

I do not think there is a small(<10 lines) code change that will make my program work for snippet 3 and all related cases that have newlines in brackets and parentheses. This is because I believe that checking for newlines and the rules for newlines are even more complicated than a regular error, as then you have to start parsing lines. I feel that on a priority list of fixes, fixes for newlines in brackets and parentheses should be near the bottom of the list.