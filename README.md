## This repo keeps the hugo-generator files. Separate from the post files.

### Create new post
When create new posts: `hugo new content/posts/{directory}/{post}.md`

I have to create post in `content/posts/` directory, otherwise, the posts won't come up in homepage.

If I want to create blog posts structure, create directory in `posts/` directory as well.

Remove the `draft: true` if publish. I didn't remove it in archtype because I think it might be a good idea to keep it there still.

### Generate content with `hugo -t {theme Name}`

I'm using `PaperMod`:
```bash
hugo -t PaperMod
```

### Push articles to repositories

When push to GitHub, push `hugo-base` and `public` directory separately.

##  post categories and tags

Category (a general topic)

- backend
- java (this will be its own directory)
- linux

Tag (any sub-category)

- command
- git
