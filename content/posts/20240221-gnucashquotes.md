---
title: "GnuCash, Apple Silicon and Online Quotes"
image: ""
date: 2024-02-21T14:43:07+01:00
draft: false
tags: ["apple", "silicon", "gnucash", "json", "error"]
---

Let us asume we are using GnuCash to track our finances. We also make use of the Price Database to track our Cryptoinvestment. Now as the title suggests, we are using an Apple Silicon based computer. Chances are that we are encountering a problem with JSON::Parse. We are unable to fetch online quotes.

The vast knowledge of the Internet gives us an answer to this problem. It seems that JSON::Parse will be installed for ARM instead of x86.

Thanks to [_nbrulois_](https://www.reddit.com/user/nbrulois/) we can quickly fix this.

## Identifying the Problem

```bash
sudo /Applications/Gnucash.app/Contents/Resources/bin/gnc-fq-updat
/Applications/Gnucash.app/Contents/MacOS/gnucash-cli --quotes info
```

## Fixing the Problem

### Remove JSON::Parse

```bash
sudo perl -MCPAN -e shell
install App::cpanminus

exit

sudo cpanm --uninstall Finance::Quote
sudo cpanm --uninstall JSON::Parse

exit
```

### Restart Terminal App with Rosetta

We need to start the _Terminal App_ with Rosetta. To do this we first need to quit the _Terminal App_.

Right click on the _Terminal App_ and select _Get Info_. Check the box _Open using Rosetta_. Close and relaunch the _Terminal App_.

```bash
sudo /Applications/Gnucash.app/Contents/Resources/bin/gnc-fq-update

/Applications/Gnucash.app/Contents/MacOS/gnucash-cli --quotes info
```

### Revert Terminal App changes

After we have verified that the issue is fixed, we can revert the changes we did to the _Terminal App_.

## Conclusion

Launch GunCash and enjoy as you are able to get your online quotes!

## Credits

Again, thanks to [_nbrulois_](https://www.reddit.com/user/nbrulois/) and his comment on [Reddit](https://www.reddit.com/r/GnuCash/comments/12c250i/comment/jh1y9ao/).
