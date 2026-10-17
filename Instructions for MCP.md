**To test on your PC:**

1. Install **Node.js 22 or newer**, if needed.  
2. Extract the ZIP to C:\\Tools\\animate-mcp.  
3. Close Animate, then run **INSTALL.cmd**. It installs dependencies and enables the unsigned development panel.  
4. Run **START-BRIDGE.cmd** and leave it open.  
5. Open Animate with a saved test FLA.  
6. Choose **Window → Extensions → Animate MCP Bridge**, then click **Connect**.  
7. Run **TEST-CONNECTION.cmd**.

Next, we need to create the tunnel for browser access:

1. Open the [Secure MCP Tunnel setup guide](https://developers.openai.com/api/docs/guides/secure-mcp-tunnels?utm_source=chatgpt.com).  
2. Follow its **Platform tunnel settings** link.  
3. Create a tunnel named **Adobe Animate**, if your account permits it. The setup also requires a runtime API key and the tunnel client on your PC. [OpenAI API](https://developers.openai.com/api/docs/guides/secure-mcp-tunnels?utm_source=chatgpt.com)

**After the tunnel is created.** Next, we need its client running on your Windows PC.

1. Click **Download tunnel-client** at the top of that page.  
2. Download the Windows build and extract it into C:\\Tools\\animate-mcp.  
   **tunnel-client-v0.0.15-windows-amd64.zip**   
3. In PowerShell, run:  
   cd C:\\Tools\\animate-mcp  
   .\\tunnel-client.exe help quickstart  
4. Create a **runtime API key** at [API keys](https://platform.openai.com/settings/organization/api-keys). Use the organization where you created the tunnel—not an Admin API key. 

5. Enter the key locally using this hidden prompt:   
   \$secureKey \= Read-Host "Paste your runtime API key" \-AsSecureString  
   \$env:CONTROL\_PLANE\_API\_KEY \= \[System.Net.NetworkCredential\]::new('', \$secureKey).Password  
   Remove-Variable secureKey  
6. Run  
   \$tunnelId \= Read-Host "Paste your tunnel ID"  
   .\\tunnel-client.exe init \--sample sample\_mcp\_stdio\_local \--profile adobe-animate \--tunnel-id \$tunnelId \--mcp-command "node C:/Tools/animate-mcp/src/server.js"  
   .\\tunnel-client.exe doctor \--profile adobe-animate \--explain  
7. Start the Tunnel  
   .\\tunnel-client.exe run \--profile adobe-animate  
8.   
   

