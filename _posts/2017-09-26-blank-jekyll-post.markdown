---
layout: post
title: "Python script to create a blank Jekyll-style blog post"
categories: python
---

A short script to create a blank post.

[Link to GitHub repo](https://github.com/jamesharrop/blog_post/blob/master/blogpost.py)

<pre class="lang:default decode:true " >
#!/usr/bin/env python3

# Creates a blank blog post in Jekyll format
# Usage: blogpost blog_post_title

from datetime import datetime
import argparse
import os.path
import sys

def create_file(*, post_title_text):
    file_path = "/Users/User/"
    now = datetime.now()
    file_name = file_path + now.strftime("%Y-%m-%d-") + post_title_text.replace(" ","-") + ".markdown"
    date_text = now.strftime("%Y-%m-%d %H:%M:%S %z")
    print("Attempting to create the file " + file_name)
    if os.path.isfile(file_name):
        print("Error: file already exists")
        sys.exit()
    file = open(file_name,"w+") 
    file.write("---\n")
    file.write("layout: post\n")
    file.write("title: \"" + post_title_text + "\"\n")
    file.write("date: " + date_text + "\n")
    file.write("categories: \n")
    file.write("---\n")
    file.close()
    print("File created")

def parse_arguments():
    parser = argparse.ArgumentParser()
    parser.add_argument("Post_title", help="Title of the blog post")
    args = parser.parse_args()
    create_file(post_title_text = args.Post_title)

def main():
    parse_arguments()

if __name__ == "__main__":
    main()

</pre>