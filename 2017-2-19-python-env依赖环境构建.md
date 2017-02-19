
```
curl -L https://raw.githubusercontent.com/yyuu/pyenv-installer/master/bin/pyenv-installer | bash
```

```
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile
```

```
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile
```

```
echo 'eval "$(pyenv init -)"' >> ~/.bash_profile
```

```
exec $SHELL
```

```
pyenv install -v 3.4.5
```

```
pyenv virtualenv 3.4.5 python-env-3.4.5-project
```

```
pyenv activate python-env-3.4.5-project
```

```
python --version
```

```
pip install Scrapy
```

```
pip install --upgrade setuptools
```