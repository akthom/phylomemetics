phylomemetics
=============

scripts for scraping gcmd and making trees.

full work described in: 
Thomer, A. & Weber, N. (2014).  The Phylogeny of a Dataset.  Paper to be presented at Annual Meetin of the American Society for Information Science and Technology, 2014.  Seattle, WA.

- most of these scripts are very simple, and were used to download and then parse html metadata downloaded from GCMD (should have downloaded XML but wanted to use some existing scripts I had). Workflow to parse metadata into :

     1. search for files and copy list of metadata records from GCMD
     2. clean up text (e.g.  delete free text description)
     3. extract URLS using method of choice (Text wrangler) -  save to txt file.  
     4. Run DownLoadListofLinks.py using saved text file from step 3.
     5. Move downloaded files into folder, make back up copy, change relevant lines of code pointing to file in crawlLinks.py
     6. Open all downloaded files in Text Wrangler, and begin text clean up. Because all of the files use html comments to mark fields, and because Beautiful Soup doesn't seem to recognize comments in any useful way, we're going to make some ad hoc xml for BeautifulSoup to work with later
          * replace all `<\!-- ` with `\</metadata><metadata tag="`   <-- this closes the prior section while opening up the new section
          * replace all `-->` with `\>`
          * grep to find and delete repeated filler like =, spaces, etc
          * finally, find and delete all line breaks (this just makes for cleaner output later).
     7. You are finally ready to crawl the text.  Run files through crawlfiles.py.  This extracts the metadata elements listed in "tags" and pushes them into a csv, thereby turning a series of XML/HTML
     8. Some additional text clean up might be necessary
     
     
Character conversion 
=============
Next step: converting the GCMD text data into 1s and 0s for PAUP to read.  

This is something that could be scripted in the future, but for now I do think it was better to do largely by hand -- character coding is tricky and requires human eyes.  Plus, it'll help you know your data a bit better.
     
There are two kinds of characters: continuous and discrete.
-Continuous characters are things like dates, times, and resolutions.  These need to be "binned" into numerical categories (e.g. all datasets created between 1960-1970 get a 1, 1971-1980 get a 2, and so on).
- Discrete characters are things like scientific parameters, which are coded 0 or 1 for absence and presence, respectively.  So, if a metadata record says the dataset includes Sea Surface Temperature readings, I create a column that with the Term: SST, and the attribute of 1, and code the remaining datasets accordingly.
- resulting dataset is COADSGCMD.nex
     
Again, this is a long and laborious process -- email thomer2 at illinois dot edu for further explanation.
     
PAUP
=============
Beyond the scope of this readme -- but the ML tree was created with Heuristic search + TBR branch switching options (per advice of Julie Allen). 

