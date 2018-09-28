| Note: Instructions assume you are using Mac OSX.

Installation
============

Python VirtualEnv
-----------------

```
python -m pip install virtualenv
python -m pip install virtualenvwrapper
```

Make sure `.profile` has:
```
export VIRTUALENVWRAPPER_PYTHON=/usr/local/bin/python3
export WORKON_HOME=$HOME/.virtualenvs
export PROJECT_HOME=$HOME/Documents/projects
source /usr/local/bin/virtualenvwrapper.sh
```

Jupyter
-------

Create a virtual environment:
```
mkvirtualenv jupyter
workon jupyter
```

Install IPython first to lock it to a specific version (this is due to a conflict between IPython and Jupyter with respect to prompt-toolkit):
```
pip install "ipython<7.0.0"
```

Install the version of prompt-toolkit that will keep both IPython and Jupyter happy:
```
pip install "prompt-toolkit<2.0.0,>=1.0.0"
```

Install Jupyter:
```
pip install jupyter
pip install jupyter-console
```

Usage
=====

Vanilla Notebook Server
-----------------------

Start the Jupyter server - open a new Terminal and run:
```
workon jupyter
jupyter notebook
```

This will open up a web-browser as per usual.

Console
-------

As an alternative, you can also run jupyter console:
```
workon jupyter
jupyter console
```

| Note that this will start its own Jupyter server.

EIN and Emacs
-------------

You can also run this in emacs using 'ein'. Suggested emacs configuration for this is:
```
(defvar setup-py-packages
  '(better-defaults
    elpy
    ein
    flycheck
    py-autopep8))

(dolist (p setup-py-packages)
  (when (not (package-installed-p p))
    (package-install p)))

(elpy-enable)

;; use flycheck not flymake with elpy
(when (require 'flycheck nil t)
  (setq elpy-modules (delq 'elpy-module-flymake elpy-modules))
  (add-hook 'elpy-mode-hook 'flycheck-mode))

(add-hook 'elpy-mode-hook
          (lambda ()
            ;; These work because shell variables are grabbed by init.el

            ;; Bog-standard python3
            ;;(setq python-shell-interpreter "python3")
            ;;(setq elpy-rpc-python-command "python3")

            ;; IPython
            (setq python-shell-interpreter "ipython"
                  python-shell-interpreter-args "-i --simple-prompt")

            ;; Restore mode removed by globabl change for clojure
            (setq electric-indent-mode t)

            ;; enable autopep8 formatting on save
            (require 'py-autopep8)
            (setq py-autopep8-enable-on-save t)))
```

If you want to do any plotting, you will have to do use the graphical emacs client and run it from the command line explicitly:
```
/Applications/Emacs.app/Contents/MacOS/Emacs
```

Then, you have two options; either:
# Connect to an existing server - use `M-x ein:notebooklist-login` and then enter the _token_ emitted from the server when you started it. Then run `M-x ein:notebooklist-open`
# Run your own server with `M-x ein:jupyter-server-start` (then wait as this will also do a notebooklist)

Matplot Example
===============

Open a Notebook and enter the following:
```
%matplotlib inline

import matplotlib.pyplot as plt
plt.style.use('seaborn-white')
plt.plot([1,2,3,4,5])
plt.ylabel('some numbers')
plt.show()
```
Then run it (`Ctrl-Enter` in the web-page or `M-x Enter` in emacs).
