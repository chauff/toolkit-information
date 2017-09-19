# toolkit-information

## [Indri](https://www.lemurproject.org/indri/)


### Installation

1) Download Indri from [Sourceforge](https://sourceforge.net/projects/lemur/files/lemur/indri-5.11/indri-5.11.tar.gz/download)

2) In the directory of the download: `gunzip indri-5.11.tar.gz`, followed by `tar -xvf indri-5.11.tar`

3) Create a directory where Indri should be installed, e.g. `mkdir /myhome/Indri`

4) Enter Indri's source directory (the unzipped/untarred one): `cd /myhome/indri-5.11`

5) Run `./configure --prefix=/myhome/Indri/` in Indri's source directory to generate the Makefiles. The prefix tells Indri in which directory to install Indri in.

6) Run `make`, followed by `make install` followed by `make clean` in Indri's source directory. If an error such as `zlib.h not found` occurs during the `make` phase, check if zlib is installed. If not, install it via `sudo apt-get install zlib1g-dev` and run `make` again, followed by the two install && clean commands.

This is it.

### Indexing Vol45 (TREC disks 4 & 5 minus CR)

In order to index a collection `IndriBuildIndex` is used; it takes one or more parameter files as input. The documentation of the parameter files can be found [here](https://sourceforge.net/p/lemur/wiki/IndriBuildIndex%20Parameters/).

For TREC disks 4 & 5 a typical `buildindex.param` file looks as follows:

```xml
<parameter>
 <corpus>
  <path>/myhome/Vol45-corpus/</path>
  <class>trectext</class>
 </corpus>
 <stemmer>
  <name>krovetz</name>
 </stemmer>
 <index>/myhome/Vol45-krovetz</index>
</parameter>
```

The `class` parameter is set according to the manner, in which the collection documents are formatted. If this class is set incorrectly (it is incompatible with the format of the collection documents), the index usually fails to be created. The stemmer here is set to the Krovetz stemmer, if the `<stemmer>` lines are removed or an invalid stemmer name is chosen, no stemming occurs. The `path` is the path to the input collection directory, and the `index` determines where the index is stored.

To start the indexing process, run `/myhome/Indri/bin/IndriBuildIndex buildindex.param`.

To check whether the indexing was successful, take a look at the `manifest` files in the created index. The command `more /myhome/Vol45-krovetz/manifest` should yield an output similar to this one:

```xml
<parameters>
	<indexCount>2</indexCount>
	<indexes>
		<index>1</index>
	</indexes>
	<injectURL>true</injectURL>
	<normalize>true</normalize>
	<stemmer>
		<name>krovetz</name>
	</stemmer>
</parameters>
```

and the command `more /myhome/Vol45-krovetz/index/1/manifest` should yield:

```xml
<parameters>
	<code-build-date>Sep 19 2017</code-build-date>
	<corpus>
		<document-base>1</document-base>
		<frequent-terms>11159</frequent-terms>
		<maximum-document>528156</maximum-document>
		<total-documents>528155</total-documents>
		<total-terms>253367449</total-terms>
		<unique-terms>664603</unique-terms>
	</corpus>
	<fields></fields>
	<indri-distribution>Indri release 5.11</indri-distribution>
	<type>DiskIndex</type>
</parameters>
```

Note that the manifest file shows you the number of documents/terms in the index (useful sanity check).


### Querying the index

It is useful to separate out the parameters for the retrieval phase (index and retrieval rules):
```xml
<parameters>
 <rule>method:dirichlet,mu:1000</rule>
 <index>/myhome/Vol45-krovetz</index>
</parameters>
```

and the parameters for the queries:
```xml
<parameters>
 <query>
  <number>101</number>
  <text>#combine(prime factors)</text>
 </query>
 <query>
  <number>102</number>
  <text>#combine(dog food)</text>
 </query>
<parameters>
```
Lets call these files `param1` and `param2`. To run the queries over the index, run: `/myhome/Indri/bin/IndriRunQuery param1 param2`. This call will print the document rankings to the terminal. All available parameters for `IndriRunQuery` can be found [here](https://lemur.sourceforge.io/indri/IndriRunQuery.html). Note that two parameter files are not strictly necessary, more or fewer parameter files can be used (as long as they are wrapped in `<parameters>`).

