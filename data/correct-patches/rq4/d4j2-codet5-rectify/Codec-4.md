# Rectify Patch

```

        this(0);
```

# Developer Patch

```
        this(0);
```

# Context

```
--- bug/Codec-4/src/java/org/apache/commons/codec/binary/Base64.java

+++ fix/Codec-4/src/java/org/apache/commons/codec/binary/Base64.java

@@ -222,7 +222,8 @@

      * </p>
      */
     public Base64() {
-        this(false);
+
+        this(0);
     }
 
     /**
```

# Note

