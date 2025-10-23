For analysis and plotting, how we need data is as arrays. 
We need to think about optimization also. 
So how about something like this: 
- There are data sources: TDMS files, Folders with TDMS files, same with TXT, DAT or IBW. 
- Then we interpret these data in a common data structure: Like a pandas dataframe or array or list or whatever optimum and convenient. As an e.g. a channel from a TDMS file and a column of a txt file can be added to the same dataframe (if we decide pandas dataframe) as columns. This in my opinion adds more flexibility and abstraction. 
- Then there's data processing: Using the data structure we've created, we can have functions applied to this data. For e.g. Differentiate one column w.r.t. another. Similarly there could be other functions (users can modularly add functions to the functions list) 
- Then there's plotting: We plot the processed/unprocessed data as a XY plot or intensity plot or whatever. Users can program the plot types also. What do you think about this architecture?

