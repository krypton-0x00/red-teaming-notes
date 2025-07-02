
### Default mode

Once we are done with the installation, we can proceed with the enumeration part. To perform a default directory brute force, we can use the following the command:

feroxbuster -u http://192.168.1.4

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjCdBWdpuQtFw1BJQfzo8b8Ibn7mBuJsdJFLAb0MgFTw6rNDGFZu1C7HN2C7MlOi7h-QUmCbb-b0pq_ueK9N7fUqs8O8eQveXMcPEtXauy3PavrSS4rZZU4b19p2eqizj9fBwe-TrHWpVO56-6t4OhW_j6lfw715jinab6nTNvHEw5BGRyPf9PnZlVCOJZr/s16000/2.png)

It can be seen from above that the wordlist used in default mode is the **raft-medium-directories.txt**.

To get a less verbose output, we can use the **–silent** flag to hide the non-essential data.

feroxbuster -u http://192.168.1.4 --silent

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgDgVL6Q524kSZu9ZCEeLEFEpa4wHR-BsPIemI9gOIPRE9J1OjTjX8Y6jcH_Hq0MF6h-4U2TpaTUrUhHBd6jpBgUX_cBteC3Wcp7p2ZljmrRUOZJ_7lzN5_ZnsU8ocQIpchrEofOIeckrdKhDH8ioVEeuM3YdATRZPNfCWPvROpNhBRtWNPACiucEZC-00x/s16000/3.png)

### Redirects

In order to allow the Feroxbuster to continue the directory brute forcing on the redirected URL, we can use the **-r** or **–redirect** flag. For example if http://192.168.1.4 redirects to http://192.168.1.4/newpath, Feroxbuster will follow this redirection and continue to scan http://192.168.1.4/newpath for directories and files.

feroxbuster -u http://192.168.1.4 -r

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjCuqYHm6dGRy7RqJA-n-PJdOKS_Jmdne_C_7IRGUQdFHdrWoYuGWayQZfo8RLBWAKlvZJoW_-mi5mHnLKNZJrMGDdUd4ZQcZfuw8nQgCOBmml2htGmC56GRqdmJNg7Su4pW7raljbPlj7_E9N6ytYE1d168LX21UBcxJvCV-9XBH7MDwjThRBmFglEU-5M/s16000/6.png)

### Extensions

To perform brute-force for a particular type of file extension, the **-x** or **–extensions** flag can be used.

feroxbuster -u http://192.168.1.4 -x php,txt --silent

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj9CL4RJ3NU9slFoNyzOiaQy-I1Tvrj9AB1faXpyk39MXnSms_XTWKr6fptKpi56h3qj8FHFd9wfh0cRNjrtde46z2xjkK0zP9oCJlBhl55Ww9QBzqaDhy11sXuW2UrZWCPn6oM0C4sl1xjAm4lxx3Fb3wNZx4WixWEvvD9rubBEmeLesffl-kcgqMce9u8/s16000/7.png)

### Result output

If we want to log the output, we use the **–output** flag and then mentioning the file name.

feroxbuster -u http://192.168.1.4 --output results.txt

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhSpooB2JCzeP25nunaVg-dKaJSe1iFogq8JnpyyWHzo1l0FNx7tuZZNswy0DnyC8e1kLpGDGZ2v4G3TcdoVae20bQHsiZgmL8G83mmXuj1finEut_Z3FHYdAwHfX0qSKJPmSUWgHIZNpCDSpshpyWdUWNmbMXVT1_rZOBlFqmZgq0LNOKtJPLYAgkHcutl/s16000/8.png)
