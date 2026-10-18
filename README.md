# `Survey of Codex Custom MCP Server`

`To create a Custom MCP Server the APPLICATION should have one workable`  
`route of the following table`

| `Application capability` | `How an MCP server can use it` |
| :---- | :---- |
| `API`  | `Read data and perform actions through HTTP or a programming library` |
| `Scripting`  | `Execute commands inside the application` |
| `Plugin or extension` | `Install a bridge that receives commands and returns results` |
| `Command-line interface` | `Run commands and collect their output` |
| `Documented file format` | `Read or modify project files, even if the application isn’t running` |
| `Only graphical interface` | `Automate clicks and keystrokes; possible, but usually more fragile` |

`The key limitation is that MCP does not create capabilities the APPLICATION lacks.`  
`For example, if an app only exposes image export, we can expose image export. However, editing individual layers would require additional methods.`

## Beyond that, we must consider

* **`A defined set of tasks:`**  
  `What should the assistant read, create, or change?`  
* **`Access and permissions:`**  
  `Any required account, API credentials, application license, or permission to install an extension.`  
* **`A place to run the server:`**  
  `Usually your computer for desktop applications, or a hosted service for web applications.`  
* **`A connection to the AI client:`**  
  `A transport the client supports, such as local standard input/output or HTTP.`   
* **`Useful feedback:`**  
  `A way to return data, report errors, and verify whether an operation succeeded.`

`This exercise surveys the following:`

1. `Launching a local MCP server that will communicate to the Application through stdio and will be served locally on a Windows computer.`  
2. `Codex/ChatGPT Work in the browser will secure a tunnel from the MCP server to OpenAI.`

`This exercise was conducted on a Windows PC although tasks should be transferable for OSX.`

`Access to the PowerShell will be used although one could transpose commands to Bash or CMD or Terminal for Mac.`

`Administrator Permissions will be required.`

# Developer Mode

**`Make sure Developer Mode is enabled in ChatGPT/Codex`**  
`For me, I found this setting under the follow:`  
`Bottom left > Settings > Security and login`  
`then look for Developer mode.`

`The layout may have changed and Developer mode may not be necessary by the time of this documentation. Check current documentation to find the proper location where this switch is located.`

# Build a Custom MCP Server

`Assuming an MCP server does not yet exist for the APPLICATION or you have specific needs such that current MCP servers are not applicable.`  
`Unless you are comfortable writing backend software, I recommend vibe coding a local MCP Server for your application using Codex.`

`A prompt like, “Help me write an MCP server for my application”, etc...`  
`The APPLICATION must support one of the above mentioned interfaces so that the AI can communicate with your application.`  
`This survey will center around a BRIDGE, plugin/extension that connects to the AI, in this case Codex, by way of a transport pathway called a TUNNEL. Tunnel client software will be provided by OpenAI.`

`The server should reside locally in a folder on your computer.`  
`Try using a folder path like:`  
`C:\Tools\[server folder name]`

`The AI should provide instructions on how to start the server.`  
`This may consist of an installation of lightweight backend software such as Node.js and Scripts and configurations files.`

**`Example Workflow:`**

1. `Run an Installation script`  
2. `Run a Start Script`  
3. `Run a Connection Script in the APPLICATION to start the BRIDGE`

# Set up a Tunnel

`A secure MCP Tunnel will be the transport pathway to private MCP servers to OpenAI’s Control Plane.`

1. `Open the OpenAI Secure MCP Tunnel Setup Guide`  
   [`https://developers.openai.com/api/docs/guides/secure-mcp-tunnels?utm_source=chatgpt.com`](https://developers.openai.com/api/docs/guides/secure-mcp-tunnels?utm_source=chatgpt.com)  
2. `Click the Platform Tunnel Settings link on the page`  
3. `On the Tunnels page, click Create Tunnel`  
4. `Enter Name and Description, leave rest as is and Submit`  
   `Name: Application Name`  
   `Description: Describe Tunnel for Application`  
5. `Copy the Tunnel ID and save in a text file`

# Set up the Tunnel Client

`The tunnel-client runs the long-lived daemon connecting the MCP server`  
`to the OpenAI Control Plane.`

1. `In the Tunnels page, click the Download Tunnel-Client Button at the top of the page next to Create Tunnel`  
2. `Click the one for your system, amd64 means 64-bit Intel or AMD`  
   `For example, tunnel-client-v0.0.15-windows-amd64.zip`  
3. `Extract the tunnel client into your server folder`  
4. `Run the client to get diagnostic data as follows.`  
   `If help documentation returns then you are ready.`  
   `cd C:\Tools\[server folder name]`  
   `.\tunnel-client.exe help quickstart`

# 

# Creating the API key

1. `Keep the Powershell window open`  
2. `Go to the OpenAI API Keys website`  
3. [`https://platform.openai.com/settings/organization/api-keys`](https://platform.openai.com/settings/organization/api-keys)  
4. `Create new Secret Key`  
   `Enter a name like: [App Name] MCP Tunnel Key`  
5. `Keep it at Default Project`  
6. `For Permissions select Restricted`  
7. `Set the Tunnels row to Use`  
8. `Leave everything else at None`  
9. `Click the Create Secret Key button`  
10. `Copy the API Key and keep it private`

# Initializing the Tunnel Client

1. `Return to the PowerShell window`  
   `cd C:\Tools\[server folder name]`  
   `$secureKey = Read-Host "Paste your runtime API key" -AsSecureString`  
2. `Paste your API key`  
3. `Now secure the API Key value and remove the from temporary memory`  
4. `$env:CONTROL_PLANE_API_KEY = [System.Net.NetworkCredential]::new('', $secureKey).Password`  
5. `Remove-Variable secureKey`  
6. `Use the Tunnel ID and Server executable to create a profile for the Tunnel Client to the MCP Server`  
7. `$tunnelId = Read-Host "Paste your tunnel ID"`  
   `.\tunnel-client.exe init --sample sample_mcp_stdio_local --profile [App Name] --tunnel-id $tunnelId --mcp-command "node C:/Tools/animate-mcp/src/server.js"`  
8. `Paste the Tunnel ID`  
9. `Check the tunnel for Diagnostic Info`  
   `.\tunnel-client.exe doctor --profile [App Name] --explain`  
10. `If it passes you can run the tunnel client long-lived daemon`  
    `.\tunnel-client.exe run --profile [App Name]`

# Verifying the local MCP Server

`We will need to make sure the MCP server is running and is connecting to the OpenAI Control Plane through the OpenAI Secure MCP Tunnel.`  
`We will need to verify the MCP server Bridge is connecting to the Application,`

`The MCP Server installation should have a Start-Bridge script or similar. There may be a "Connect" button from within the app via an Extension or Script depending on the Application capability.`

`Finally we and most importantly we will need to Add a Custom MCP Server to the Codex Plugins library,`

1. `Plugins > Add > Add custom MCP Server`  
   `Enter Name and Description`  
2. `Click Tunnel`  
3. `Enter the Tunnel ID here`  
4. `No Authentication (since its local we should be safe)`  
5. `Follow the instructions to start the Server`  
6. `Test the MCP Server. Go to the Codex prompt and Enter`  
   `@[Application Name]`  
   `Prompt to Application Diagnostic Request`  
7. `If all goes well Congratulations!`  
   `If not you will need to request help from AI`
