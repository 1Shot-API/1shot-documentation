..  youtube:: YiwjpvvsE5U
   :align: center

.. raw:: html

   <br />


MCP
===

1Shot API can be connected to any agentic coding environment that allows MCP server connections with OAuth dynamic client registration. This includes Cursor, Replit, and other popular agentic coding environments.

The 1Shot API MCP server allows coding agents to configure your imported smart contract methods, server wallets, x402 integrations and stablecoin payments automatically.

Connecting to the MCP Server
----------------------------

Add the following to your MCP configuration:

.. code-block:: json

   {
     "mcpServers": {
       "1Shot API": {
         "url": "https://mcp.1shotapi.com/mcp",
         "transport": "streamableHttp",
         "auth": {
           "CLIENT_ID": "P5Jduw80vpVAINgW8lnNwgak9ALgfBIS",
           "scopes": ["openid", "profile", "email", "offline_access"]
         }
       }
     }
   }

Install on Replit
~~~~~~~~~~~~~~~~~~

.. raw:: html

   <div class="mcp-install-wrap">
     <a href="https://replit.com/integrations?mcp=eyJkaXNwbGF5TmFtZSI6IjFTaG90IEFQSSIsImJhc2VVcmwiOiJodHRwczovL21jcC4xc2hvdGFwaS5jb20vbWNwIn0=">
       <img src="https://replit.com/badge?caption=Install%201Shot%20API%20MCP" alt="Install 1Shot API MCP on Replit">
     </a>
   </div>

Click the "Install" button below to add the 1Shot API MCP server to your Replit environment. Using the Replit coding agent, you can build and host web3 applications in one spot, letting 1Shot API handle the server wallet and payment infrastructure.

Install in Cursor
~~~~~~~~~~~~~~~~~

.. raw:: html

   <div class="mcp-install-wrap">
     <div class="cursor-mcp-install-btn">
     <a href="cursor://anysphere.cursor-deeplink/mcp/install?name=1Shot%20API&amp;config=eyJ1cmwiOiJodHRwczovL21jcC4xc2hvdGFwaS5jb20vbWNwIiwidHJhbnNwb3J0Ijoic3RyZWFtYWJsZUh0dHAiLCJhdXRoIjp7IkNMSUVOVF9JRCI6IlA1SmR1dzgwdnBWQUlOZ1c4bG5Od2dhazlBTGdmQklTIiwic2NvcGVzIjpbIm9wZW5pZCIsInByb2ZpbGUiLCJlbWFpbCIsIm9mZmxpbmVfYWNjZXNzIl19LCJoZWFkZXJzIjp7fX0%3D">
       <img src="https://cursor.com/deeplink/mcp-install-dark.svg" class="cursor-mcp-img-light" alt="Install 1Shot API MCP in Cursor">
       <img src="https://cursor.com/deeplink/mcp-install-light.svg" class="cursor-mcp-img-dark" alt="Install 1Shot API MCP in Cursor">
     </a>
     </div>
   </div>

Add the 1Shot API MCP server to your Cursor environment for creating web3 applications and agents locally: 

1Shot Prompts
-------------

There are millions of smart contracts across the EVM ecosystem, some of them are better documented than others. `1Shot Prompts <https://app.1shotapi.com/1shot-prompts>`_ lets you turn any verified EVM smart contract into an annotated tool that can be accessed via 1Shot API's MCP server. This improves the reliability and LLM token utilization of coding agents when constructing coding logic around your target smart contracts.

Smart Contracts as Tools 
~~~~~~~~~~~~~~~~~~~~~~~~~~

Smart contract prompts can be turned into tools by importing them into your `business </basics/businesses-and-teams.thml>`_. You can then list all the tools available in your business by `listing the contract methods </api/api.html#list-available-contract-methods>`_ in your business. The description fields of the contract methods and their inputs and outputs can be passed to an LLM to help it reason about how to use the contract method and what to expect from the transaction execution.

Smart contracts must have verified ABI and source code available on Etherscan in order to be published as a prompt. A prompt is a collection of human readable text that describes how the smart contract is to be used in a specific context plus descriptions of the contract functions that would be used in that context along with descriptions of the function inputs and outputs. A single smart contract can have multiple prompts, each describing a different use case or context for the contract.
