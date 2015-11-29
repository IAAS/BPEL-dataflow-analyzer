# BPELDataFlowAnalyzer

This program implements a data flow analysis algorithm on BPEL processes as described in the following scientific works:

 * Kopp, Oliver; Khalaf, Rania; Leymann, Frank: Reaching Definitions Analysis Respecting Dead Path Elimination Semantics in BPEL Processes, Technical Report No. 2007/04, University of Stuttgart, 2007. http://www2.informatik.uni-stuttgart.de/cgi-bin/NCSTRL/NCSTRL_view.pl?id=TR-2007-04&mod=0&engl=1&inst=IAAS
 * Kopp, Oliver; Khalaf, Rania; Leymann, Frank: Deriving Explicit Data Links in WS-BPEL Processes. In: Proceedings of the International Conference on Services Computing, 2008. http://dx.doi.org/10.1109/SCC.2008.122
 * Breier, Sebastian: Extended Data-flow Analysis on BPEL Processes, Diploma Thesis 2726, IAAS, University of Stuttgart, 2008.  http://www.informatik.uni-stuttgart.de/cgi-bin/NCSTRL/NCSTRL_view.pl?id=DIP-2726&engl=1
 * Gao, Yangyang: Implementierung einer Datenflussanalyse f�r WS-BPEL 2.0, Student Thesis 2246, IAAS, University of Stuttgart, 2010. http://www2.informatik.uni-stuttgart.de/cgi-bin/NCSTRL/NCSTRL_view.pl?id=STUD-2246&engl=1

See also http://www.iaas.uni-stuttgart.de/.

## Introduction
You should definitely read DIP-2726 to understand the purpose and internal working of the program.
The program uses the [Eclipse BPEL Designer] project's EMF (Eclipse Modeling Framework) model to analyze BPEL
processes.

This README will explain how to use the software and how to build and extend it yourself.

Unfortunately, the software has some limitations and is still unfinished.
However, most activity types are supported.

This program depends on:

- Java 6
- git clone of [Eclipse BPEL Designer]. Commit 43b50022c77f09fcec19019d09b7e82063ce9772 (of 2015-08-20) works. Import following projects:
  - org.eclipse.bpel.common.model
  - org.eclipse.bpel.model
- [BPEL-model-utilities](https://github.com/IAAS/BPEL-model-utilities) - de.uni_stuttgart.iaas.bpel.model.utilities
- [BPEL4Chor-model](https://github.com/IAAS/BPEL4Chor-model) - org.bpel4chor.model
- Following dependencies should be automatically be resovled:
  - org.eclipse.emf.common
  - org.eclipse.emf.ecore
  - org.eclipse.wst.wsdl
  - org.eclipse.emf.ecore.xml

## Use it using Eclipse

1. Install [Eclipse](http://www.eclipse.org/downloads/). Currently, the program runs on [Eclipse Mars (aka version 4.5.1) JEE version](http://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/mars1).
2. Pull the source for the [Eclipse BPEL Designer]
  See http://www.eclipse.org/bpel/install.php on how to do this.
  Basically, connect to :pserver:anonymous@dev.eclipse.org:/cvsroot/technology,
  check out from org.eclipse.bpel/plugins:
  - org.eclipse.bpel.common.ui
  - org.eclipse.bpel.model
  - org.eclipse.bpel.common.model
  Check out the HEAD revisions.
3. Import the BPELDataFlowAnalyzer project into Eclipse.
4. Import the test project (de.uni-stuttgart.iaas.bpel-d.test) into Eclipse
5. For a test run, run "BPEL Dataflow Analyzer Test"
6. In case you want to analyze other models, check de.uni_stuttgart.iaas.bpel_d.test.Start

## Command-line run without Eclipse support (unsupported)

1. Edit the Build Path of BPELDataFlowAnalyzer.
 * Add the checked out parts of Eclipse BPEL project.
 * Add the org.eclipse.emf.* and org.eclipse.wst.wsdl dependencies as described above.
 * The JAR files can be found in the eclipse/plugins directory.
2. Edit "Analyzer.java" method "main" to contain the right paths and file names to your BPEL process and the variable list file.
3. Run "Analyzer.java" as Java application.

## Restrictions

- Containment of variable elements is done using string matching

### Not implemented are

- Fault Handlers
- Compensation Handlers

## Extend it
When you got it to run, you can also extend the program easily.

Some words about architecture:

- The Analyzer class is just startup code which loads the BPEL file through Eclipse BPEL project and
starts analysis.
- The classes in the infrastructure package are necessary to encapsulate the analysis algorithm
  infrastructure, such as saving the writer state, objects for saving activity flags, and so on.
- The classes in the analysis package are the actual analysis algorithms presented in DIP-2726.

Class "State" is a singleton that saves the state of the analysis.
It maps "Placements" to write tuples (consisting of poss, dis, inv, mbd, as in the thesis).
Placements are tuples consisting of one activity and an enum instance "IN", "OUT", or "TEMP".
This way, it is possible for "State" to save and retrieve writes_in/writes_out/writes_tmp tuples
for each activity.
"State" also maps activities to a set of flags.
Also, it saves variables to analyze.
"State" also contains various methods for retrieving and saving the described data.
Please see usage examples of the "State" class in many methods for more information.

analysis.Utility contains several interesting utility methods:

- dumpSet will pretty-print a set of ExtensibleElements (activity and process elements)
- dumpEE will pretty-print an ExtensibleElement
- others

## Copyright and License
 * Copyright 2008 Sebastian Breier
 * Copyright 2010 Gao Yangyang and Oliver Kopp

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.

   [Eclispe BPEL Designer]: https://eclipse.org/bpel/