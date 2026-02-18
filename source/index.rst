.. 1shot-documentation documentation master file, created by
   sphinx-quickstart on Wed Feb 12 19:54:45 2025.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

.. image:: ./_static/welcome-banner-light.png
   :alt: 1Shot API welcome banner (light theme)
   :align: center
   :class: only-light

.. image:: ./_static/welcome-banner-dark.png
   :alt: 1Shot API welcome banner (dark theme)
   :align: center
   :class: only-dark

.. raw:: html

   <br />

Welcome to 1Shot API
====================

..  youtube:: qsawSR3tOSs
   :align: center

.. raw:: html

   <br />

`1Shot API <https://1shotapi.com>`_ is a full-stack web3 infrastructure platform that lets you bring reliable onchain agents, workflows and products to market fast with account abstraction, server wallets and consumer stablecoin payments. 1Shot API offers MCP capabilities to connect to the most popular agentic coding tools like Cursor or Replit to prompt your way to a production-ready onchain application or agent.

The fastest way to get started is to connect the `1Shot Prompts </prompts/index.html>`_ MCP server to your favorite agentic development environment like Cursor or Replit. The agentic coding environment will handle setting up server wallets and importing the necessary smart contract methods into your account.

Getting Started
----------------------------------

Make a free 1Shot API account at `app.1shotapi.com <https://app.1shotapi.com>`_. Here are the core features you will use to get started with the 1Shot API:

.. grid:: 2 2 2 2
    :gutter: 2

    .. grid-item-card:: 1. Businesses and Teams 🏢
        :link: /basics/businesses-and-teams.html
        :link-alt: Businesses and Teams 

        Create a business, add team members, and manage billing.

    .. grid-item-card:: 2. Server Wallets 👛
        :link: /basics/wallets.html
        :link-alt: Wallets

        Provision and fund 1Shot API server wallets for relaying transactions.

    .. grid-item-card:: 3. Stablecoin Payments 💰💳
        :link: /1shotpay/index.html
        :link-alt: Consumer Payments with 1ShotPay

        Integrate 1ShotPay into your application to accept consumer zero-fee stablecoin payments for carts and agentic payments.

    .. grid-item-card:: 4. AI-Development Tools 🤖
        :link: /prompts/index.html
        :link-alt: 1Shot Prompts MCP Server

        Connect 1Shot API to your favorite agentic development environment to prompt your way to a production-ready onchain application or agent.
   
    .. grid-item-card:: 5. Calling Smart Contracts 📝
        :link: /basics/contract-methods.html
        :link-alt: Calling Smart Contracts

        Read from and write to smart contracts by importing their methods into your business's API.

    .. grid-item-card:: 6. Calling the 1Shot API 💻🐀
        :link: api/api.html
        :link-alt: Calling the 1Shot API

        Use your API key and secret to trigger transactions from your application.

How It Works
------------

.. image:: ./_static/1Shot-API-How-It-Works-Light.png
   :alt: 1Shot API how it works (light theme)
   :align: center
   :class: only-light

.. image:: ./_static/1Shot-API-How-It-Works-Dark.png
   :alt: 1Shot API how it works (dark theme)
   :align: center
   :class: only-dark

.. raw:: html

   <br />

1Shot API is not an RPC provider, it is a customizable transaction relayer service which handles the full transaction lifecycle with real-time webhook callbacks on the final state of your application's transactions. 1Shot API allows you to read from and write to smart contracts without needing to import web3 clients or smart contract ABIs into your source code. This lets you keep your workflows and application logic simple and focused on your product's unique value proposition.

The 1Shot API service is designed to handle heavy request traffic. If your product has many users generating onchain actions all at once, 1Shot API ensures all of your transactions will make it to the chain quickly and gas efficiently without flooding the mempool. 1Shot API greatly simplifies the technical overhead of adding digital assets or on-chain logic to any application, bot, or agent, regardless of the language your application is written in. Additionally, with its powerful team & role management features, 1Shot API can scale with your product as your team and user base grows from proof-of-concept to enterprise scale.

Several helpful client SDKs for popular languages like `Python <https://pypi.org/project/uxly-1shot-client/>`_, `Typescript <https://www.npmjs.com/package/@uxly/1shot-client>`_ are available so you can one shot your next app in no time, leaving the complexities of delegation, transaction batching, submission and monitoring to us.

.. toctree::
   :hidden:
   :maxdepth: 2

   basics/index.rst
   1shotpay/index.rst
   prompts/index.rst
   x402/index.rst
   automation/index.rst
   api/index.rst
