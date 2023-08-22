# Rectify Patch

```
        final int bottom = stack.size() - 1;
```

# Developer Patch

```
        final int bottom = stack.size() -1;
```

# Context

```
--- bug/Jsoup-68/src/main/java/org/jsoup/parser/HtmlTreeBuilder.java

+++ fix/Jsoup-68/src/main/java/org/jsoup/parser/HtmlTreeBuilder.java

@@ -465,10 +465,7 @@

 
     private boolean inSpecificScope(String[] targetNames, String[] baseTypes, String[] extraTypes) {
         // https://html.spec.whatwg.org/multipage/parsing.html#has-an-element-in-the-specific-scope
-        int bottom = stack.size() -1;
-        if (bottom > MaxScopeSearchDepth) {
-            bottom = MaxScopeSearchDepth;
-        }
+        final int bottom = stack.size() - 1;
         final int top = bottom > MaxScopeSearchDepth ? bottom - MaxScopeSearchDepth : 0;
         // don't walk too far up the tree
```

# Note

