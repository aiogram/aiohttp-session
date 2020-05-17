Aiohttp extension for aiogram
#############################

Using aiogram with aiohttp as http client

AiohttpSession represents a wrapper-class around `ClientSession` from `aiohttp <https://pypi.org/project/aiohttp/>`_

Installation
============

::

    pip install aiogram_aiohttp_session


Basic Usage example
===================

.. code-block:: python


    from aiogram import Bot
    from aiogram_aiohttp_session import AiohttpSession

    session = AiohttpSession()
    Bot('token', session=session)


Proxy requests in AiohttpSession
================================

In order to use AiohttpSession with proxy connector you have to install `aiohttp-socks <https://pypi.org/project/aiohttp-socks/>`_

Create proxy session
--------------------

.. code-block:: python

    from aiogram import Bot
    from aiogram_aiohttp_session import AiohttpSession

    session = AiohttpSession(proxy="protocol://host:port/")
    Bot(token="bot token", session=session)

.. note:: Only following protocols are supported: http(tunneling), socks4(a), socks5 as aiohttp_socks documentation claims.


Authorization
-------------

Proxy authorization credentials can be specified in proxy URL or come as an instance of `aiohttp.BasicAuth` containing
login and password.

Consider examples:

.. code-block:: python

    from aiohttp import BasicAuth
    from aiogram_aiohttp_session import AiohttpSession

    auth = BasicAuth(login="user", password="password")
    session = AiohttpSession(proxy=("protocol://host:port", auth))
    # or simply include your basic auth credential in URL
    session = AiohttpSession(proxy="protocol://user:password@host:port")


.. note:: Library prefers `BasicAuth` over username and password in URL, so
    if proxy URL contains login and password and `BasicAuth` object is passed at the same time
    aiogram will use login and password from `BasicAuth` instance.


Proxy chains
------------

Since `aiohttp-socks <https://pypi.org/project/aiohttp-socks/>`_ supports proxy chains, you're able to use them in aiogram

Example of chain proxies:
^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: python

    from aiohttp import BasicAuth
    from aiogram.api.client.session.aiohttp import AiohttpSession

    auth = BasicAuth(login="user", password="password")
    session = AiohttpSession(
        proxy={"protocol0://host:port0",
               "protocol1://user:password@host:port1",
               ("protocol2://host:port2", auth),}  # can be any iterable if not set
    )


Rationale
=========

Get rid of huge redundant code-base in main repo. Make an example of session extension for aiogram
