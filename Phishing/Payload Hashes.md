```bash
for i in $(ls | grep -v testing-index.html);do echo $i $(md5sum $i | cut -d " " -f 1) $(sha256sum $i | cut -d " " -f 1) >> tmp;done;sed '1 i Name MD5 SHA256' tmp | tabulate -1 > payload-hashes.txt; rm tmp
```