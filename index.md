---
layout: default
---

[Back to main page](https://ornaka.github.io)

This is my writeup about TryHackMe's PSEclipse-room, which is centered around Splunk

Link to the room 

https://tryhackme.com/room/posheclipse



# Scenario

Scenario : You are a SOC Analyst for an MSSP (Managed Security Service Provider) company called TryNotHackMe .
A customer sent an email asking for an analyst to investigate the events that occurred on Keegan's machine on Monday, May 16th, 2022 . The client noted that the machine is operational, but some files have a weird file extension. The client is worried that there was a ransomware attempt on Keegan's device. 
Your manager has tasked you to check the events in Splunk to determine what occurred in Keegan's device. 
Happy Hunting!

## First Question:

A suspicious binary was downloaded to the endpoint. What was the name of the binary?

Since there are Sysmon logs available, I started by searching eventcode 3, which is used for network connections. Checking the DestinationIP-field, one stood out, which could be the adversaries IP address.

![Potential IP address of the adversary](/images/pic2.png)

After adding the IP address to the query, I almost immediately found the most likely name of the binary, but had  to be sure. Since I assumed the binary was downloaded via PowerShell, I queried only Powershell, checked the commandline field and found the encoded command for downloading the binary.  Decoding it in CyberChef confirmed that it was indeed the binary I saw earlier. 

![Encoded command](/images/pic3.png)


### Header 3

```js
// Javascript code with syntax highlighting.
var fun = function lang(l) {
  dateformat.i18n = require('./lang/' + l)
  return true;
}
```

```ruby
# Ruby code with syntax highlighting
GitHubPages::Dependencies.gems.each do |gem, version|
  s.add_dependency(gem, "= #{version}")
end
```

#### Header 4

*   This is an unordered list following a header.
*   This is an unordered list following a header.
*   This is an unordered list following a header.

##### Header 5

1.  This is an ordered list following a header.
2.  This is an ordered list following a header.
3.  This is an ordered list following a header.

###### Header 6

| head1        | head two          | three |
|:-------------|:------------------|:------|
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |

### There's a horizontal rule below this.

* * *

### Here is an unordered list:

*   Item foo
*   Item bar
*   Item baz
*   Item zip

### And an ordered list:

1.  Item one
1.  Item two
1.  Item three
1.  Item four

### And a nested list:

- level 1 item
  - level 2 item
  - level 2 item
    - level 3 item
    - level 3 item
- level 1 item
  - level 2 item
  - level 2 item
  - level 2 item
- level 1 item
  - level 2 item
  - level 2 item
- level 1 item

### Small image

![Test if smaller pictures work properly as large imaged don't](/images/pic1.png)

### Large image

![Picture 1](/images/pic1.png)



### Definition lists can be used with HTML syntax.

<dl>
<dt>Name</dt>
<dd>Godzilla</dd>
<dt>Born</dt>
<dd>1952</dd>
<dt>Birthplace</dt>
<dd>Japan</dd>
<dt>Color</dt>
<dd>Green</dd>
</dl>

```
Long, single-line code blocks should not wrap. They should horizontally scroll if they are too long. This line should be long enough to demonstrate this.
```

```
The final element.
```
