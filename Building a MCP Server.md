The documented location is:

1. Open ChatGPT Plugins.  
2. Click the Add button to create a developer-mode app.  
3. Add Custom MCP Server  
4. Add name, description  
5. Select the Tunnel option

Create the Tunnel  
    Next, we need to create the tunnel for browser access:

1. Open the Secure MCP Tunnel setup guide. https\://developers.openai.com/api/docs/guides/secure-mcp-tunnels?utm\_source=chatgpt.com  
2. Click Platform tunnel settings link.  
3. Create a tunnel named Adobe Animate, if your account permits it. Add description. Leave Organizations and ChatGPT Workspaces. You will need the tunnel ID later

The setup also requires a runtime API key and the tunnel client on your PC. OpenAI API  
Tell me what the tunnel settings page shows—especially whether you can create a tunnel. Keep any API key private.

Creating an API Key

1. Open OpenAI API Keys  
   https\://platform.openai.com/settings/organization/api-keys  
2. Click Create new secret key in the upper-right corner.  
3. If asked who owns it, choose You.  
4. Name it Animate MCP Tunnel.  
5. If asked for a project, choose the project you intend to use—Default project is fine if that’s your only one.  
6. There’s a Tunnels row near the bottom, between Batch and Datasets.  
7. Click its None dropdown. Leave all other permissions set to None.  
8. If it offers Read \+ Use, select that or Use..Leave other permissions at None.   
9. Click Create secret key.