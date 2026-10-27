.. _scp06:

======================================
SCP06: Improper first match extraction
======================================

What it does
============

Finds uses of ``getall()[0]``, ``[0].getall()``, ``extract_first()`` and
similar with :class:`~scrapy.Selector` and
:class:`~scrapy.selector.SelectorList` that can be replaced
:meth:`~scrapy.selector.SelectorList.get`.

``[0].get()`` and ``[0].extract()`` are only reported without arguments, since
selector getters take none: with arguments they are something else, e.g. an
lxml element, where ``get()`` reads an attribute.


Why is this bad?
================

:meth:`~scrapy.selector.SelectorList.get` is cleaner and more efficient.


Example
=======

.. code-block:: python

    import scrapy


    class MySpider(scrapy.Spider):
        name = "myspider"

        def parse(self, response):
            yield {"title": response.css("h1::text").getall()[0]}

Use instead:

.. code-block:: python

    import scrapy


    class MySpider(scrapy.Spider):
        name = "myspider"

        def parse(self, response):
            yield {"title": response.css("h1::text").get()}


Fix
===

Uses of ``extract_first()`` are automatically fixable with the ``--fix``
command-line option.
