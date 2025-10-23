Previously I've been using manim community edition.
But since Grant was uploading tutorial videos since recently, I thought of switching to his original version of manim.

Here I'll explain the installation of manimgl- which is the 3b1b original version of manim.

As a first step I created a dedicated folder for manim.
Then I created a virtual environment inside the folder.
Then I installed manimgl using:
```
pip install manimgl
```
Apart from this I also installed MikTex and OpenGL

Then I had to do manimgl --config

From the videos repo of grant, I grabbed the file manim_imports_ext and the folder named 'custom'

interactive running
self.embed() is needed
manimgl filename scenename

rendering
manimgl filename scenename -w
can do --prerun as well

