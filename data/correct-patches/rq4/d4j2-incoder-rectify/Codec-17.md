# Rectify Patch

```
        String charset = "ISO-8859-1";
        return newString(bytes, charset);
```

# Developer Patch

```
        return newString(bytes, Charsets.ISO_8859_1);
```

# Context

```
--- bug/Codec-17/src/main/java/org/apache/commons/codec/binary/StringUtils.java

+++ fix/Codec-17/src/main/java/org/apache/commons/codec/binary/StringUtils.java

@@ -336,7 +336,8 @@

      * @since As of 1.7, throws {@link NullPointerException} instead of UnsupportedEncodingException
      */
     public static String newStringIso8859_1(final byte[] bytes) {
-        return new String(bytes, Charsets.ISO_8859_1);
+        String charset = "ISO-8859-1";
+        return newString(bytes, charset);
     }
 
     /**
```

# Note

