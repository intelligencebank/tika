# Tika App build for command line (with pdfbox jai-imageio sqlite-jdbc ocr)
## Build(Tika 1.24.1)
- Install openjdk11
- Install Maven3
- Download tika full src
- Edit tika-parsers/pom.xml and remove the following scopes for jai-imageio & sqlite-jdbc
```
<scope>test</scope>
<scope>provided</scope>
```
- Edit tika-parsers/src/main/java/org/apache/tika/parser/ocr/TesseractOCRParser.java, comment everything in `checkInitialization`
- `mvn install  -Dossindex.fail=false -DskipTests=true`
- jar output in tika-app/target/tika-app-*.jar
- On the server `apt-get install tesseract-ocr tesseract-ocr-eng tesseract-ocr-fra` ...(and all the other useful languages, simply put `tesseract-ocr-all` for all the available languages)

## Build(Tika 2.8.0)
- Install openjdk (20 as of this writing)
- Install Maven3 (3.9.4 as of this writing)
- Download tika full 2.8.0 src and unzip
- Update tika-parent/pom.xml to update dependency versions or they will fail audit due to vulnerabilities
  - <sqlite.version>x.xx.x.x</sqlite.version> -> <sqlite.version>3.42.0.0</sqlite.version>
  - <bouncycastle.version>x.xx</bouncycastle.version> -> <bouncycastle.version>1.76</bouncycastle.version>
  - <snappy-java>x.x.x.x</snappy-java> -> <snappy-java>1.1.10.3</snappy-java>
- tika-parsers have been split into various modules based on types and `jai-imageio` `<scope>test</scopes>` are to be removed from
  - tika-parsers/tika-parsers-standard/tika-parsers-standard-modules/tika-parser-pdf-module/pom.xml
  - tika-parsers/tika-parsers-standard/tika-parsers-standard-modules/tika-parser-image-module/pom.xml
  - tika-parsers/tika-parsers-standard/tika-parsers-standard-package/pom.xml
- Edit `tika-parsers/tika-parsers-standard/tika-parsers-standard-modules/tika-parser-ocr-module/src/main/java/org/apache/tika/parser/ocr/TesseractOCRParser.java`, comment everything in `checkInitialization`
- `mvn install  -Dossindex.fail=false -DskipTests=true`
- jar output in tika-app/target/tika-app-*.jar
- On the server `apt-get install tesseract-ocr tesseract-ocr-eng tesseract-ocr-fra` ...(and all the other useful languages, simply put `tesseract-ocr-all` for all the available languages) Note: this is currently already done in the dockerfile
- `ocr_config.xml` options are currently set to what will pass on `content_extraction_test.exs` tests 

## Exec
### Default (no ocr nor extractInlineImages)
```
java -jar tika-app.jar --config=default-config.xml --help
java -jar tika-app.jar --config=default-config.xml --text /path/to/doc.pdf
```

### OCR (ocr with extractInlineImages)
```
java -jar tika-app.jar --config=ocr-config.xml --help
java -jar tika-app.jar --config=ocr-config.xml --text /path/to/doc.pdf
```
https://cwiki.apache.org/confluence/display/tika/TikaOCR
