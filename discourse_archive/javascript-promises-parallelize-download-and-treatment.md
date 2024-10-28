# Javascript promises: parallelize download and treatment

## Edern_Haumont on 2018-07-30T11:33:56.192Z

Hi everyone,


A few months ago, I asked about a way to send VTI files by block ([here](https://discourse.girder.org/t/send-data-by-block/120)). We came up with a working solution : it is possible, using specific features of vtk XML formats, to send a compressed volume by blocks to a client. Then it does all the job of decompressing blocks of data, putting them at the right spot in the final dataset and doing the rendering.


My question is about the client so it may be a bit off\-Girder\-topic but I do not really know if it belongs somewhere else either.


Currently, the client code to retrieve blocks sequentially (explained [there](https://developers.google.com/web/fundamentals/primers/promises#creating_a_sequence)) looks like that:



```
// Loop through blocks
requestedBlocks.reduce((sequence, blockIndex) => {
	// Add these actions to the end of the sequence
	return sequence.then(() =>{
	    // Download
	    return sendAPIRequest(
		volume.blocksURL.blocksDownloadURL,
		requestMethods.get,
		{
		    blockIndex: blockIndex,
		},
	    );
	}).then((response) => {
	    // Treatment
	    reader.readBlock(response, blockIndex);
	});
}, Promise.resolve());

```

I attached to the topic a profiling screenshot. w[![Screenshot%20from%202018-07-30%2013-20-06](https://discourse.girder.org/uploads/default/optimized/1X/3e43df5ad39524ac773ba8efd4941ad839118432_2_690x392.png)
Screenshot from 2018\-07\-30 13\-20\-06\.png1853×1053 131 KB](https://discourse.girder.org/uploads/default/original/1X/3e43df5ad39524ac773ba8efd4941ad839118432.png "Screenshot from 2018-07-30 13-20-06.png")  

You can see that sometimes a block is downloading and sometimes some computations are done with the received block, but never both at the same time. Do you have any idea on how to modify the code so that the next blocks can be downloaded by the browser while we treat the current one ?


Thanks a lot.  

Edern


---

## Jonathan_Beezley on 2018-07-30T13:13:36.581Z

There is probably a simpler way to do it, but I *think* this does what you want.



```
function request(index) {
    return sendAPIRequest(
        volume.blocksURL.blocksDownloadURL,
        requestMethods.get,
        {
            blockIndex: index,
        }
    );
}       
        
requestedBlocks.reduce((sequence, blockIndex) => {
    return sequence.then((response) =>{
        let promise;
        if (blockIndex + 1 < requestedBlocks.length) {
            promise = request(blockIndex + 1);
        }
        reader.readBlock(response, blockIndex);
        return promise;
    })  
}, Promise.resolve());

```

---

## Edern_Haumont on 2018-07-30T14:51:28.701Z

Thanks for your answer Jonathan.  

I tried a very similar flow (note that requestedBlocks is not always \[0, 1, 2… n] but a random permutation of it. My bad, that was not very clear) :



```
function requestNextBlock(blockIndex){
	return sendAPIRequest(
		volume.blocksURL.blocksDownloadURL,
		requestMethods.get,
		{
			blockIndex: blockIndex,
		},
	);
}

let blockIterator = 0;

requestedBlocks.reduce((sequence, blockIndex) => {

	return sequence.then((response) => {

		let promise;

		if (blockIterator + 1 < requestedBlocks.length) {
			promise = requestNextBlock(requestedBlocks[blockIterator + 1]);
		}

		reader.readBlock(response, blockIndex);
		blockIterator++;

		return promise;
	});
}, Promise.resolve(requestNextBlock(requestedBlocks[0])));

```

Unfortunately I got similar results (the download and “readBlock” parts are never running at the same time). I am afraid that what I ask is not possible and that I should use more complex features like web workers.


---

## Jonathan_Beezley on 2018-07-30T15:09:49.403Z

The best you can do with this is make the next request synchronously with `readBlock`. If that isn’t working well enough, I think you will have to offload the processing to an async Web Worker to get any real improvement.


---

