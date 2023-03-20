# Introduction 

**BETA SUPPORT ONLY - MAC OS SUPPORT ONLY**

The Heimdallr IDA plugin exposes a localhost gRPC server for each IDA instances which allows the [Heimdallr client](https://github.com/interruptlabs/heimdallr-client) to navigate to locations in IDA.

# Installation


1. Install ida with pip ([ensure the pip you are using matches the python environment IDA is using](#IDA-doesnt-pick-up-dependencies))
    - Using git directly `pip3 install -e git+https://git@github.com/interruptlabs/heimdallr-ida.git#egg=heimdallr_ida`
    - From a cloned repo `pip3 install -e .`
2. Launch IDA and enter the following into the console
```
import heimdallr
heimdallr.install()
```
3. Relaunch IDA and verify gRPC server sucessfully started up. You should something like the following in the output console:
```
[Heimdallr RPC] Plugin version 0.0.1
Starting server on 127.0.0.1:51278
Wrote {"pid": 36813, "address": "127.0.0.1:51278", "file_name": "example.i64", "file_hash": "b058de795064344a4074252e15b9fd39"} to /Users/roberts/.idapro/heimdallr/36813
```
4. Install [heimdallr-client](https://github.com/interruptlabs/heimdallr-client)

# Usage

You should now be able to open ida:// URIs from anywhere in the system. This could be a Slack DM, a Confluence page, or a Obsidian note. The format is as follows (these):
`ida://example.i64%3Foffset%3D0x1002315b6%26hash%3Db058de795064344a4074252e15b9fd39%26view%3Ddisasm`
These are automatically generated by creating a note in the `heimdallr_ida` plugin

The search behaviour for a relevant IDB is as follows:
1. Search for an open IDA instance with this database already open
2. Search IDA recently open files for the location of the database
3. Search your `idb_path` for matching files

The search pattern is used to ensure links can be used easily within a team - so long as you have a database based on the same source file and is named the same.

IDBs are matched by both the database name and source file hash. As such **changing the database name will cause URIs to no longer be valid**. 

You can make notes by highlighting the area of text in IDA you want to copy and pressing "Ctrl+Shift+N". The text will be added to a code block with a link back to where it came from and added to your clipboard.

If you want to make a link to share with someone else, pressing "Ctrl+Alt+N", and the link to where you are in IDA will be added.

This currently only works for the Disassembly and Pseudocode views.

# Common Issues

## IDA doesn't pick up dependencies

The version of Python IDA is using can be different from that of your system. To verify the version of python you are using you can put the following in the IDA Python console.
```
Python>import os
Python>os.__file__
'/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/lib/python3.9/os.py'
```
And doing the same in the terminal
```
roberts@RobertS-IL-Mac Documents % python3 -q
>>> import os
>>> os.__file__
'/opt/homebrew/Cellar/python@3.10/3.10.8/Frameworks/Python.framework/Versions/3.10/lib/python3.10/os.py'
```

From here you can either update IDA to match with:
`/Applications/IDA\ Core\ 8.1/idabin/idapyswitch`

Or install the packages into the releveant python enviroment by using the path from IDA:
`/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/bin/python3 -m pip install -e  git+ssh://git@github.com/interruptlabs/heimdallr-ida.git#egg=heimdallr_ida`
