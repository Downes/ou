# oer-md-publish

Template repo for autobuilding OpenLearn sites via nbsphinx using Github Actions and publishing them via Github Pages. 

*Which means what exactly?*

It means you have access to OpenLearn content as simple text files that you can easily edit (i.e. repurpose) and publish via your own website using a free hosting service...

It also means you can play with the content on MyBinder...

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/psychemedia/openlearn-publish-test/master)

## So how do I start?

This template repository can be used as a basis for creating websites derived from OpenLearn content that is avialble in the OU-XML format (which is most OpenLearn units / courses).

To create your own OpenLearn content site, you need to get a Github account. You can [sign up here](https://github.com/join?source=header-home). Github is owned by Microsoft, so you will be giving your personal data to them...

When you are logged in to Github, generate a copy of this template repository by clicking the green *Use this template* button or [clicking here](https://github.com/psychemedia/openlearn-publish-test/generate).

Find yourself a nice [OpenLearn course unit](https://www.open.edu/openlearn/free-courses/full-catalogue). When you've found one you'd like to, erm, appropriate, you need to grab its web address / URL.

In parrticular, you're looking for a URL that __ends__ in something like ``/content-section-0` or `/content-section-0?active-tab=description-tab` (the section number doesnlt matter...).

For example, something like:

`https://www.open.edu/openlearn/science-maths-technology/learn-code-data-analysis/content-section-overview-0?active-tab=description-tab learn_to_code`

Copy the URL.

In your repository, go to the `Issues` tab](./issues) and create a new issue by clicking on the green *New issue* button. (Alternativley, [this link](./issues/new) may do it?)

In the title box, enter:

`Fetch https://www.open.edu/openlearn`

In the main body of the comment, on the first line, paste the URL you copied.

Click the green *Submit new issue* button and go and grab a cup of tea (the first build make take five to ten minutes).

__If I have set things up properly, only issues created by you or a collaborator on the repo should trigger any data grabbing and website building activity. If you submit another issue, it will essentially reset the repo, so any changes you have made to content will be lost.__

When you come back, refresh your repository homepage. Click on the `content` directory link in your repository file list (or [click here](`./tree/master/content`) and you should see some file directories that contain simple markdown text files that have been generated from the source file for the OpenLearn content.

These files should also have been rendered elsewhere in the repository as HTML web pages (you donlt need to know where...). You should be able to see the website rendered from them on a URL with the pattern:

`https://YOUR_GITHUB_USERNAME.github.io/YOUR_REPO`

So for example, this repo is:

`https://github.com/psychemedia/openlearn-publish-test/`

and the associated website is at:

`https://psychemedia.github.io/openlearn-publish-test/`

If the website *isn't* there, you may need to give the Github Pages website builder a prod. Go to your repository settings page from the *Settings* tab on the repo toolbar (or [click here](./tree/master/content)) and scroll down until you see the __Github Pages__ heading. Their *should* be a green highlighted link to your pages there. If there *isn't*, in the *Source* dropdown list select `master branch` and then select `gh-pages branch`. This should kick things back into action. After a minute or two, refresh the page and a green backgounded link to you website should be there.

If you click on the link and the OpenLearn materials *aren't* there, force a reload of the webpage in your browser to clear any cached versions of the page.

## And What's the Binder thing?

MyBinder is part of a the Jupyter ecosytem. Clicking the button will cause MyBinder to launch a Jupyter notebook server that looks onto an environment created from this repository.

*When you clone the repo, the button will still point to my repo. I havenlt got round to creating a Github Action to fix that yet... Feel free to edit the `README.md` file on your repository to fix it yourself, or copy your repo URL and paste it into the box at [mybinder.org](https://mybinder.org) to launch your repo via the MyBinder homepage.*

When the Binderised environment is built, and the notebook server launched (you shuld be redirected to it automatically), navigate into the `content` directory from the the notebook homepage, and then to one of the markdown files; click on one of the markdown file links. The document will open up in the interactive, read/write Jupyter notebook user interface. Which means you can edit it, execute any code in it, add and execute code of your own.

*Unfortunately*, there's no safe and direct way of saving your content back to the repository. Instead, you'll have to click on the `Download` button in a notebook toolbar and then open your browser onto the appropriate directory in your repo, and drag the file you just downloaded onto that directory listing. You should see an upload view to upload the files. Commit them (with an explanatory note about them) and the build and republish should be kicked into action again.


## Changing the content

From the Github repository homepage, you can navigate to any or the source markdown webpages and click on the pencil icon (`Edit this file`) button at the bottom of the page. When you are happy with your changes, click the green *Commit changes* button at the bottom of the page. You may also want to edit a *commit message* to describe the change(s) your made.

When you commit the change, a new HTML version of the page will be generated and published to your Github Pages website.

There's a lot more things possible, but I need to write some more docs to explain exactly what...


## So how does it work?

Short answer: *string'n'glue*.

Longer answer: when you submit this issue, it triggers a Github Action, (a bit like a helpful brownie of folklore fame) that grabs a "source code" version of the OpenLearn unit (an XML document in the OU-XML format). A Python package ([open-ouxml-tools](https://github.com/innovationOUtside/open-ouxml-tools)) is then used to convert this XML to markdown document and places them in the `content` directory, along with an index generating page (`index.rst`) in the top level of the directory. These files are then automatically *committed* and *pushed* the top level of the repository.

The action then uses the Sphinx documentation generation tool to render the markdown pages as HTML. In fact, a lot of magic is done using the `nbsphinx` package with a bit of help from the Jupytext package as part of the conversion. The actual conversion process is `md -> Jupyter notebook (ipynb) -> HTML`. The reason for doing this is that we can edit the markdown in a rich Jupyter notebook style environment to preview various interactive things that we can also render to HTML. It also means that we can execute Python code included in the markdown documents and render the outputs from that code execution (which might include charts, for example, or data tables) in the final HTML. But that needs another set of docs to explain properly...

Once the HTML has been generated, some more Github Action magic copies the HTML pages to where they need to be so that they're viewable as the final, published Github Pages website.