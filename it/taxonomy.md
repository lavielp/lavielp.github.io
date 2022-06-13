# Taxonomy

How do we organize documentation?  Traditionally we have built a single, monolithic,
hierarchical taxonomy.  The problem with a single hierarchy is they tend to make
sense to the one person who created it, it's generally inflexible and does evolve
over time.

An alternative is a single, unstructured repository that requires tags of some sort.
A tagging system will allow for a set of documents to be reordered to a new view
based on the tags.  The problem here is tags tend to be open and unenforcable, and
people must learn the set of necessary tags.

Within IT, there are a few well known document types such as network diagrams, flow
charts and UML diagrams.  Many of these are programmer and project management
artifacts.  There are very few, if any, well defined documents for the design and
architecture of IT operations and infrastructure, the network diagram be the possible
sole exception.

IT operations and infrastructure professionals often have 4 different views of the IT
landscape: Knowledge Base, Network, Server, Application.  As a problem is addressed
and a solution developed, the view may change.  When troubleshooting a web site failure,
for example, the engineer may need to understand the application view to understand all
of the servers involved, look at individual servers to understand how they which 
critical components are installed, and perhaps finally the network view if connectivity
needs to be managed.  Finally they may need to write a knowledge base article if the
ultimate issue is something an operations or help desk person can address next time.

Several specific document formats emerge from those 4 views.  A knowledge base will
contain a "break fix" document and a "how to" document.  The network view will be a
traditional network diagram which can show both an application centric view and
server centric view.  A server document which contains specific information such as
IP address, name, installed applications and components.  And an application document
which addresses the overall application components which may include servers and
network connectivity.

