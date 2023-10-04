# Group Website

### Building the Site
First step:
```
pip install sphinx
sphinx-build -b html source/ build/html
```

### Adding a Post
1) Add your project named `MyProject` to `index.rst` under the `Project Catalog` block:
2) Add a File `MyProject.rst` under `source/`

After any modifications to your project (`source/MyProject.rst`) :
```
sphinx-build -b html source/ build/html
```

Here is a Cheetsheet for ``.rst``:
<a href="https://bashtage.github.io/sphinx-material/rst-cheatsheet/rst-cheatsheet.html" target="_blank">CheetSheet</a>



