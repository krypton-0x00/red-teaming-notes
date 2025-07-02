DEPS: 

```
pip3 install scrapy
```

```sh
shakir0x00@htb[/htb]$ wget -O ReconSpider.zip https://academy.hackthebox.com/storage/modules/144/ReconSpider.v1.2.zip

```


```USAGE
python3 ReconSpider.py http://inlanefreight.com
```

will generate a `results.json` file

| JSON Key         | Description                                                            |
| ---------------- | ---------------------------------------------------------------------- |
| `emails`         | Lists email addresses found on the domain.                             |
| `links`          | Lists URLs of links found within the domain.                           |
| `external_files` | Lists URLs of external files such as PDFs.                             |
| `js_files`       | Lists URLs of JavaScript files used by the website.                    |
| `form_fields`    | Lists form fields found on the domain (empty in this example).         |
| `images`         | Lists URLs of images found on the domain.                              |
| `videos`         | Lists URLs of videos found on the domain (empty in this example).      |
| `audio`          | Lists URLs of audio files found on the domain (empty in this example). |
| `comments`       | Lists HTML comments found in the source code.                          |
