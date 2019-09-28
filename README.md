# pypykatz_wasm
The [pypykatz](https://github.com/skelsec/pypykatz) project's LSASS and Registry HIVE parsing capability is now in you web browser!  

# !THE PARSING IS IN YOUR BROWSER, NOT VIA WEB!

# I'm just here for the result
Slow down Morris.  
Files are hosted [here](https://ppk.thu.gg/) by [thugcrowd](https://thugcrowd.com/)  
The compiled versions are at [releases](https://github.com/skelsec/pypykatz_wasm/releases) 

# How does it work
There is an awesome project called [pyodide](https://github.com/iodide-project/pyodide) which aims to have a fully working python3 interpreter running in webassembly.  
Webassembly in a nutshell allows your c/c++/go/... code to be compiled to a binary file which the JS engine in your browser can execute. Interfacing it via javascript (creeps me out tho)  
The pyodide framework allows additional python packages to be included, so I made some extensions to pypykatz to make it play better with webassemblies interfaces (mostly adding byte-input based parsing) and asked ppl from [thugcrowd](https://thugcrowd.com/) to design a cool looking webui for it. 
When opening the page the JS kicks in and loads python which includes all necessary modules to run pypykatz. Then you'll need to point it to the files you wish to parse. This will make the browser to read the file, store it in a JS variable, which will get passed to the python engine and then pypykatz. Easy. Pypykatz will then parse the file, store the output JSON in a global variable then switch back to the JS engine in your browser which will render the results from said variable.

# I don't trust you
You really shouldn't, hence the code is public you can read and verify it yourself.  
For the lesser-concerned, run it in an offline environment.

# How to compile it
You have two options. Either use the code from this repo directly or from the official pyodide repo. Using the version I have will make life easier as it already has the necessary packages for pypykatz, the official repo will need to be modified a bit.  
In case you decide to use the official repo for compiling and it fails for you, please ask them to help you out because I will not.

## Using the current repo
- clone the repo
- go to the pyodide folder
- cat readme, go to the how to compile it via docker section. Srsly, use the docker I wasted a day trying to compile it without that.
- do what it tells you to do
- after the compiling is done you will get the pyodide/build folder full of files.
- add the html file created by thugcrowd for your viewing pleasure
- either host it with a python oneliner `python3 -m http.server` or open the html file by double-clicking it (be careful the latter might not work depending on your browser's settings)

## Using the official pyodide repo
- clone the repo
- add `pypykatz`, `aoiwinreg`, `minikerberos`, `minidump` to  `pyodide/packages` folder structure in the appropriate format (you can check this repo's folders and copy them)
- cat readme, go to the how to compile it via docker section. Srsly, use the docker I wasted a day trying to compile it without that.
- do what it tells you to do
- after the compiling is done you will get the pyodide/build folder full of files.
- add the html file created by thugcrowd for your viewing pleasure
- either host it with a python oneliner `python3 -m http.server` or open the html file by double-clicking it (be careful the latter might not work depending on your browser's settings)

# Will this eat a shitton of RAM?
You bet it will. Basically all files parsed will need to be first made available to the webassembly engine via JS. Then stored in memory until the parsing finishes. Then wait until garbage collection, which lets be honest here can happen between now and forever.

# The build files are huge, can it be smaller?
Probably yes. You may want to delete unnecessary python packages from `pyodide/packages` however I cannot guarantee that it will work at all. For example numpy looks really well-embedded even in hte non-python parts of this project.
