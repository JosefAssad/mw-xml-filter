This script functions as a git filter for versioning MediaWiki XML exports.

To use it:

1. Assumming you use the file extension .mwxml for MediaWiki XML dumps:
2. Place this script somewhere on the $PATH and ensure it is executable
3. ensure you have the lxml python library installed
4. Add a line in .gitattributes like `*.mwxml diff=mediawiki`
5. Execute `git config diff.mediawiki.textconv mw-xml-filter.py`

How it looks
============

As a simple example, we diff between two MEdiaWiki XML exports. The older one contains one page called TestPage001. The newer version has two changes

1. TestPage001 has had an extra line added
2. New page TestPage002 has been created with one line of text and one Category assignment

The git diff without this script looks as follows:

```diff
diff --git a/structure.xml b/structure.xml
index 8f66fad..8d7e858 100644
--- a/structure.xml
+++ b/structure.xml
@@ -39,17 +39,39 @@
     <ns>0</ns>
     <id>809</id>
     <revision>
-      <id>978</id>
-      <timestamp>2015-06-05T11:40:53Z</timestamp>
+      <id>981</id>
+      <parentid>978</parentid>
+      <timestamp>2015-06-05T11:44:27Z</timestamp>
       <contributor>
         <username>Josef</username>
         <id>1</id>
       </contributor>
-      <comment>Created page with &quot;[[Category:TestDiff]]  Lorem ipsum.&quot;</comment>
-      <text xml:space="preserve" bytes="35">[[Category:TestDiff]]
+      <text xml:space="preserve" bytes="60">[[Category:TestDiff]]
 
-Lorem ipsum.</text>
-      <sha1>ooip0v8ejgrf3bxz3nmpbk4vwe93tu3</sha1>
+Lorem ipsum.
+
+and again, lorem ipsum.</text>
+      <sha1>rt8ib0fgk16wgbysul6cnhwwhlikcth</sha1>
+      <model>wikitext</model>
+      <format>text/x-wiki</format>
+    </revision>
+  </page>
+  <page>
+    <title>TestPage002</title>
+    <ns>0</ns>
+    <id>810</id>
+    <revision>
+      <id>980</id>
+      <timestamp>2015-06-05T11:44:14Z</timestamp>
+      <contributor>
+        <username>Josef</username>
+        <id>1</id>
+      </contributor>
+      <comment>Created page with &quot;[[Category:TestDiff]]  sometihng something&quot;</comment>
+      <text xml:space="preserve" bytes="42">[[Category:TestDiff]]
+
+sometihng something</text>
+      <sha1>6wwj2b10dwei6654xczfqq9hmsi3rj5</sha1>
       <model>wikitext</model>
       <format>text/x-wiki</format>
     </revision>

```

The git diff AFTER activating this filter looks like this:

```diff
(mw-xml-filter)(master)josef@debian:repo$ git diff -U10
diff --git a/structure.xml b/structure.xml
index 8f66fad..8d7e858 100644
--- a/structure.xml
+++ b/structure.xml
@@ -1,5 +1,11 @@
 PAGE: TestPage001
 [[Category:TestDiff]]
 
 Lorem ipsum.
 
+and again, lorem ipsum.
+PAGE: TestPage002
+[[Category:TestDiff]]
+
+sometihng something
+
```

This script is licensed under the [GPLv3](http://www.gnu.org/licenses/gpl-3.0.en.html).
