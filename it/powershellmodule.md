# PowerShell Module Structure

PowerShell modules at their most basic are just a psm1 file with a bunch of functions.  The file is
imported into a PowerShell sessions and then the functions can be called.  From there, the module can
expand and become more useful.

The next step is to have a module manifest whose file type is psd1 and contains a structure hash
table with meta data about the module such as the name, version and public functions to make
available from the associated psm1 file when the module is imported.

From there, the person authoring the module has a decision to make, either keep all the functions in
the monolithic psm1 file, or split the functions out into separate files.  There seems to be a trade
off; keeping the monolithic psm1 file has performance benefits, splitting the files is easier to
modify and maintain.

Splitting the modules and combining them when publishing is the happy middle ground.  A publishing
script can be written which reads all the separate files and appends them to a single psm1 file
which is then shipped.  This allows the module to be organized as a taxonomy which is designed with
humans and maintainability in mind.

Making sure the separate files are combined and tested after the combination process becomes a
critical step in the deployment process.  The result, though, realizes both the benefits of a single
psm1 file and human centric taxonomy.

Ultimately, the module taxonomy is a compromise between computer performance and human readability
for maintenance.  An ideal compromise on those two dimensions can be achieved with complexity on the
dimension of packaging the module.

## Current Taxonomy

This is the current taxonomy I've landed on which is a collection of ideas that have been gathered
from the PowerShell community.  The major components are common and supported by modules that have
been released or adopted and supported by Microsoft.  Dozens of examples with minor variations can
be found by browsing Github.  Figure 1 illustrates the directory structure.

|
|_docs
|_Tests
|_
