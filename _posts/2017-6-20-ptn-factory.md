---
layout: post
title: Pattern: factory
date:   2017-06-22 15:18:01 +0100
categories: pattern factory
tag: pattern
---

* content
{:toc}

## Archive

```python
from zipfile import ZipFile
import tarfile
import os


class BaseArchive(object):

    EXTENSION = None

    def __init__(self, location_path, files_to_pack):
        self.location_path = location_path
        self.files_to_pack = files_to_pack

    def generate(self):
        raise NotImplementedError()

    @classmethod
    def check_extension(cls, extension):
        return extension == cls.EXTENSION


class ZipArchive(BaseArchive):

    EXTENSION = '.zip'

    def generate(self):
        with ZipFile(self.location_path, 'w') as zip_file:
            for _file in self.files_to_pack:
                zip_file.write(_file)


class TarArchive(BaseArchive):

    EXTENSION = '.tar'

    def generate(self):
        with tarfile.open(self.location_path, 'w') as tar_file:
            for _file in self.files_to_pack:
                tar_file.add(_file)


class ArchiveManager(object):
    ARCHIVE_ENGINES = [ZipArchive, TarArchive]

    def __init__(self, location_path, files_to_pack):
        self.location_path, self.extension = os.path.splitext(location_path)
        self.files_to_pack = files_to_pack
        self.archive_engine = self.choose_archive_engine()

    def choose_archive_engine(self):
        for engine in self.ARCHIVE_ENGINES:
            if engine.check_extension(self.extension):
                return engine(self.location_path, self.files_to_pack)

    def create_archive(self):
        self.archive_engine.generate()

if __name__ == '__main__':
    am = ArchiveManager('/tmp/demo.tar', ['/tmp/a.txt'])
    am.create_archive()
```

## Operator

```python
class BaseOperator(object):

    def __init__(self, a, b):
        self.a = a
        self.b = b

    def do(self):
        raise NotImplementedError()


class Plus(BaseOperator):

    def do(self):
        return self.a + self.b


class Minus(BaseOperator):

    def do(self):
        return self.a - self.b


class Multiple(BaseOperator):

    def do(self):
        return self.a * self.b


class Divide(BaseOperator):

    def do(self):
        return self.a / self.b


class OperatorFactory(object):

    __store__ = {
        '+': Plus,
        '-': Minus,
        '*': Multiple,
        '/': Divide,
    }

    def __init__(self, a, b, o):
        self.engine = self.__store__[o](a, b)

    def calc(self):
        return self.engine.do()


def main():
    operator = OperatorFactory(1, 2, '+')
    result = operator.calc()
    assert result == 3
    operator = OperatorFactory(1, 2, '-')
    result = operator.calc()
    assert result == -1
    operator = OperatorFactory(1, 2, '*')
    result = operator.calc()
    assert result == 2
    operator = OperatorFactory(1, 2, '/')
    result = operator.calc()
    assert result == 0


if __name__ == '__main__':
    main()
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
