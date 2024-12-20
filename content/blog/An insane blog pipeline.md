---
note-type:
title: An insane blog pipeline
draft: false
tags:
  - blog
  - testInProgress
date_created: 2024-12-06
date_modified: 2024-12-10
---

# An insane blog pipeline

## The Setup

- create a folder labeled Posts. This is where you will add your blog posts
- get the path to it

### Setting up Hugo

Install Hugo
Prerequisites
Git
Go

git submodule add -f https://github.com/panr/hugo-theme-terminal.git themes/terminal

git submodule add -f https://github.com/imfing/hextra themes/hextra

### sync

```
rsync -av --delete "/Users/nicolaskogane/Library/Containers/co.noteplan.NotePlan-setapp/Data/Library/Application Support/co.noteplan.NotePlan-setapp/Notes/Posts" "/Users/nicolaskogane/10 - Projects/NKGblog/content/"
	↳ il faut que je change des chose spour que l apartie /mir soit effective
```

hugo server -t hextra → ne foncitonne pas avec hextra pour l'instant mais très bien avec terminal
↳ il faut que j'investigue

His work is making this possible:
!![Image Description](/images/Pasted%20image%2020241210100856.png)

```
- [ ] import os
- [ ] import re
- [ ] import shutil

- [ ] # Paths
- [ ] posts_dir = "/Users/nicolaskogane/10 - Projects/NKGblog/content/posts"
- [ ] attachments_dir = "/Users/nicolaskogane/Library/Containers/co.noteplan.NotePlan-setapp/Data/Library/Application Support/co.noteplan.NotePlan-setapp/Notes/@Attachments"
- [ ] static_images_dir = "/Users/nicolaskogane/10 - Projects/NKGblog/static/images"

- [ ] # Step 1: Process each markdown file in the posts directory
- [ ] for filename in os.listdir(posts_dir):
    - [ ] if filename.endswith(".md"):
        - [ ] filepath = os.path.join(posts_dir, filename)

        - [ ] with open(filepath, "r") as file:
            - [ ] content = file.read()

        - [ ] # Step 2: Find all image links in the format ![Image Description](/images/Pasted%20image%20...%20.png)
        - [ ] images = re.findall(r'\[\[([^]]*\.png)\]\]', content)

        - [ ] # Step 3: Replace image links and ensure URLs are correctly formatted
        - [ ] for image in images:
            - [ ] # Prepare the Markdown-compatible link with %20 replacing spaces
            - [ ] markdown_image = f"![Image Description](/images/{image.replace(' ', '%20')})"
            - [ ] content = content.replace(f"[[{image}]]", markdown_image)

            - [ ] # Step 4: Copy the image to the Hugo static/images directory if it exists
            - [ ] image_source = os.path.join(attachments_dir, image)
            - [ ] if os.path.exists(image_source):
                - [ ] shutil.copy(image_source, static_images_dir)

        - [ ] # Step 5: Write the updated content back to the markdown file
        - [ ] with open(filepath, "w") as file:
            - [ ] file.write(content)

- [ ] print("Markdown files processed and images copied successfully.")
```

To authenticate to github using ssh you have to use a predefined key or generate a new one, here's the link to a perfect post on this topic:
https://medium.com/codex/git-authentication-on-macos-setting-up-ssh-to-connect-to-your-github-account-d7f5df029320
to test the connection, you have to run this command in the terminal:

```
ssh -T git@github.com
```

You should get a message saying you succesfully authenticated
Git won't have shell access, which is normal
If you get a remote access problem, (https://stackoverflow.com/questions/20840012/ssh-remote-host-identification-has-changed)
the bruteforce solution is to delete the known_hosts (tryed and verified) but you can also more elegantly use

```
ssh-keygen -R <host>
```

#e.g. ssh-keygen -R 192.168.3.10

https://medium.com/codex/git-authentication-on-macos-setting-up-ssh-to-connect-to-your-github-account-d7f5df029320
