# Tika App build for command line (with pdfbox jai-imageio sqlite-jdbc ocr)
## Build(Tika 1.22)
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
- On the server `apt-get install tesseract-ocr tesseract-ocr-eng tesseract-ocr-fra` ...(and all the other useful languages, simply put `tesseract-ocr-all` for all the available languages)

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
