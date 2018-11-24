# CUTEXT - Cvalue Used To EXtract Terms
---------------------------------------

### Introduction
----------------
The heavy use of medical terms has motivated the construction of large terminological resources for English, 
such as the Unified Medical Language System (UMLS) or the Open Biological and Biomedical Ontology (OBO) ontologies. 
Purely manual construction of terminological resources is by itself very valuable, 
but it constitutes a highly time-consuming process, it does not guarantee that included concepts or terms do actually 
align with the medical language and terms as they are being used in clinical documents by healthcare professionals and 
it requires constant update and revision due to changes and emergence of new biomedical concepts over time. 

CUTEXT is a multilingual medical term extraction tool. It allows extracting terms in texts written in 
English, Spanish, Galician, and Catalan.

The main characteristics of CUTEXT are the following:
* It is implemented in java, so it is multiplatform. It has been tested under Windows and Linux.
* It is multilingual: It has been tested in English, Spanish, Catalan, and Galician.
* It can be adapted easily to other languages by simply changing the lexical tag text file configuration.
* The entry documents can be in plain text or in pdf.
* It can be executed in graphic mode or by console (command line).
* It supports numerous configuration parameters, among the most important: the language, the tagger, the 
frequency and c-value thresholds, and the entry of the document/s.
* The output is provided in plain text, in JSON format or/and in [BioC](http://bioc.sourceforge.net/).

A more detailed description of the system can be found in the journal [*Sociedad Española para el Procesamiento del Lenguaje Natural*.](http://dx.doi.org/10.26342/2018-61-5)


### Prerequisites (Dependency)
------------------------------

CUTEXT requires to have [TreeTagger](http://www.cis.uni-muenchen.de/~schmid/tools/TreeTagger/) installed on your computer.
If you are going to use medical or biomedical texts it is also convenient, although not necessary, to
install [GeniaTagger](http://www.nactem.ac.uk/GENIA/tagger/), or another specific tagger of this domain.

The route must also be included in the PATH variable, up to the TreeTagger "bin" folder.

To convert texts written in pdf to txt, we use a script that uses the class "ExtractText" of the Apache pdfbox API,
which is packaged in the "pdfbox-app-2.0.5.jar" file inside into the "jar_pdf" folder.
Therefore, the path to this file must be included in the CLASSPATH variable.
Finally, you must also include in the CLASSPATH variable the path to the "cutext" folder,
since CUTEXT is packaged at that folder.

### Directory structure
---------
CUTEXT directory structure corresponds to a package nomenclature called *cutext*
Therefore, all packages are within that folder:
* cutext/config_files/: includes files with tags, stop-words, and punctuation marks in Spanish, Galician, Catalan, and English.
* cutext/filter/lin/: contains the Java classes that implement the linguistic filter.
* cutext/filter/sta/: contains the Java classes that implement the statistical filter.
* cutext/gui/: contains the Java classes that implement the graphical user interface (GUI).
* cutext/in/: a possible place to put the input file.
* cutext/intern/TT/in/: internal storage of the input file for treetagger.
* cutext/intern/TT/out/: internal storage of the output file generated by treetagger.
* cutext/intern/TT/x/: internal storage of the intermediate file for treetagger.
* cutext/jar_pdf/: contains the "pdfbox-app-2.0.5.jar" file to convert texts written in pdf to txt. 
* cutext/main/: includes the main classes of CUTEXT as well as the file cutext.jar.
* cutext/out/fileTextHashTerms/: store the text output files.
* cutext/out/serHashTerms/: stores serialized objects. 
* cutext/postagger/: contains the class that invokes the tagger (TreeTagger). 
* cutext/prepro/: contains the classes that preprocess the input corpus. 
* cutext/properties/: contains the CUTEXT properties file.
* cutext/stemmer/: contains the classes that allow you to obtain the stem of the words.
* cutext/textmode/: contains the classes that allow CUTEXT to be executed from the terminal.
* cutext/util/: contains utility classes.



### Usage
---------
CUTEXT allows its execution in graphic mode or in text mode.
In both cases, it is assumed that it will be executed from the "main" folder.
If not, change the paths in the properties file "cutext.properties",
and include the path of this file as an input parameter when invoking CUTEXT.

To execute CUTEXT in graphic mode:
<pre>
java cutext.main.ExecCutext
</pre>

To execute CUTEXT in text mode:
<pre>
java cutext.main.ExecCutext -TM [Options] <-inputFile fileName>
</pre>

Except for the input file, all options have default values, so it is not necessary to include them.

Options:
<pre>
-TM
	Execute CUTEXT in text mode (TM).
-help
 	Show the line to execute CUTEXT, and the options.
-displayon <boolean>
	Show the messages at the standard output. Default TRUE (show).
-postagger <postagger>
	POS tagger to tagger the input file. TreeTagger (default) or GeniaTagger.
-language <language>
	SPANISH (default) or ENGLISH, CATALAN, GALICIAN.
-frecT <integer>
	Frecuency Threshold. Default 0.
-cvalueT <double>
	C-Value Threshold. Default 0.0.
-bioc <boolean>
	Create a BioC output. Default false.
-json <boolean>
	Create a JSON output. Default false.
-convert <boolean>
	If true then convert the input file into lower case. Default true.
-withoutcvalue <boolean>
	If true then execute only the linguistic filter. Default false.
-incremental <boolean>
	If true then execute one line of the file as a entire corpus. Default false.
-generateTextFile <boolean>
	If true then create one text file per hashTerms, from 'a' to 'z'. Also create a raw text file with terms sorted by cvalue. Default false.
-routeHashTerms <string>
	Folder where you want to store the hash terms.
-routeTextFileHashTerms <string>
	Folder where you want to store the text file hash terms.
-routeconfigfiles <string>
	Folder where it stores config files.
-routeinterntt <string>
	Temporary folder (TT).
-inputFile <string>
	The document to use.
-outputFile <string>
	The file to write the result to.
</pre>


### Examples
------------
Let's assume an input file "in.txt", in the folder "in", if we execute CUTEXT in text mode:
<pre>
java cutext.main.ExecCutext -TM -generateTextFile true -inputFile ../in/in.txt
</pre>
This generates the text files at the folder "cutext/out/fileTextHashTerms" and at "cutext/out/serHashTerms" the serialized terms.

If you want also to obtain outputs in the BioC and JSON formats, then you will have to execute CUTEXT by setting these parameters to TRUE, as in:
<pre>
java cutext.main.ExecCutext -TM -generateTextFile true -bioc true -json true -inputFile ../in/in.txt
</pre>


### Execution via JAR file
-----------------------------------
The cutext.jar file allows to execute CUTEXT directly from a terminal such as cmd, terminator, etc.
To do this, you have to write the following command line (from the directory where cutext.jar is located):
<pre>
java -jar cutext.jar [options]
</pre>
Where *options* are those shown in the 'Usage' section.
For example, if we type:
<pre>
java -jar cutext.jar
</pre>
CUTEXT will run the graphical interface.

On the other hand, to execute CUTEXT in text mode and with the same parameters as those included in the first example of the previous section, we must type: 
<pre>
java -jar cutext.jar -TM -generateTextFile true -inputFile ../in/in.txt
</pre>


The cutext.jar file is found at cutext/main/cutext.jar

If we change cutext.jar to another directory, we must change the properties file accordingly, which is at: 
<pre>
cutext/properties/cutext.properties
</pre>

Finally, we would like to mention that in the input folder ('cutext/in/in.txt') we include an example of a medical extract, in Spanish. Also in the output folder ('cutext/out/fileTextHashTerms/hsimpli.txt' and at 'cutext/out/fileTextHashTerms/terms_raw.txt') we include the output that CUTEXT generates for that input text. In case you want to reproduce the results obtained, CUTEXT was executed with the following parameters to generate the output text (from cutext/main/):
<pre>
java -jar cutext.jar -TM -incremental true -generateTextFile true -inputFile ../in/in.txt
</pre>


## Contact
----------
jsantamaria@cnio.es


### License
----------

(This is so-called MIT/X License)

Copyright (c) 2017-2018 Secretaría de Estado para el Avance Digital (SEAD)

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

