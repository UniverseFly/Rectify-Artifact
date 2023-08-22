# Rectify Patch

```

        return getChromosomes().iterator();
        //return super.iterator();
```

# Developer Patch

```
        return getChromosomes().iterator();
```

# Context

```
--- bug/Math-34/src/main/java/org/apache/commons/math3/genetics/ListPopulation.java

+++ fix/Math-34/src/main/java/org/apache/commons/math3/genetics/ListPopulation.java

@@ -206,6 +206,8 @@

      * @return chromosome iterator
      */
     public Iterator<Chromosome> iterator() {
-        return chromosomes.iterator();
+
+        return getChromosomes().iterator();
+        //return super.iterator();
     }
 }
```

# Note

