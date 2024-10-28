# File chunk upload using curl: 400 Bad Request

## Jean-Christophe_Fill on 2021-10-12T21:53:31.321Z

To support uploading file from a server with no python available, I am failing to upload by chunk using curl.


I currently get the following error



```
400 Bad Request
The request entity could not be decoded. The following charsets were attempted: ['utf-8']

```

The server is <https://data.kitware.com>


I suspect I am missing a flag when invoking curl … and before I spend more time on this, does anyone have some hints ?


# Details


## Authenticate using API key and obtain a Girder token



```
API_KEY=<xxxx>
curl -d "key=$API_KEY" -X POST -d "" https://data.kitware.com/api/v1/api_key/token

```

## Initiate upload



```
token=<token>
parentId=<parentID>
filename=/path/to/large_file

filesize=$(stat -f%z $filename)

echo "$filename [$filesize]"

curl -k -H "Girder-Token: $token" -X POST -d"parentType=folder&parentId=$parentId&name=$filename&size=$filesize" "https://data.kitware.com/api/v1/file"

```

it returns the `<uploadId>`



```
{"_id": "<uploadId>", "assetstoreId": ... }

```

## Split large file into chunk


*Chunk filenames are named like `xaa`, `xab`, …*



```
MAX_CHUNK_SIZE=$(expr 1024 \* 1024 \* 64)
split -b $MAX_CHUNK_SIZE $filename

```

### Upload chunks



```
uploadId=<uploadId>
offset=0
for file_chunk in $(ls -1 x*); do
  size=$(stat -f%z $file_chunk)
  echo "Uploading file_chunk[$file_chunk] offset[$offset] size[$size]"
  curl -k -H "Girder-Token: $token" -X POST -d@$file_chunk "https://data.kitware.com/api/v1/file/chunk?offset=$offset&uploadId=$uploadId"
  echo "STATUS: $?"
  offset=$(expr $offset + $size)
done

```

it returns the following



```
Uploading file_chunk[xaa] offset[0] size[67108864]
<h2>400 Bad Request</h2>
<p>The request entity could not be decoded. The following charsets were attempted: ['utf-8']</p>
<p><i>Powered by <a href="https://girder.readthedocs.io">Girder</a></i></p>
                                                                              

```

---

