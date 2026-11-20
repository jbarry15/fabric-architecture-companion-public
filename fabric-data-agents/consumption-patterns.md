# Fabric Data Agent consumption patterns

Use this page to choose where users consume governed Fabric answers. For data-agent integrations, scope, test, and publish the agent first. Some Fabric IQ experiences use Power BI semantic models directly and don't require a published data agent.

## Architecture verdict

Treat a published Fabric data agent as a reusable governed analytical tool. Keep the agent narrow, describe it clearly, preserve user-level permissions where required, and validate the consuming surface because responses can leave the Fabric compliance boundary or geography.

## Consumption surfaces

| Surface | Best use | Architecture watch-out |
|---------|----------|------------------------|
| **Microsoft Copilot Studio** | Add the published data agent as a Fabric IQ Data MCP tool when a business copilot needs governed analytical answers alongside workflow actions, topics, and other tools. | Copilot Studio can use **User** or **Maker** authentication. User authentication preserves each user's Fabric/source permissions; Maker authentication runs through the maker's access and needs explicit security approval. |
| **Microsoft Foundry via Fabric IQ / OneLake Catalog** | Let a Foundry agent discover and call one or more published Fabric data agents from the OneLake Catalog. | Preview capability. The data agent must be published, same-tenant access must work, and users need permissions to the data agent and underlying sources. |
| **Earlier Microsoft Foundry Fabric tool** | Migrate existing integrations to the Fabric IQ / OneLake Catalog tool. | The earlier integration is unsupported as of November 6, 2026; see the [retirement notice](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-foundry). |
| **MCP endpoint** | Use the published agent's MCP endpoint for custom MCP clients, VS Code, or agent frameworks that support MCP over streamable HTTP. | The endpoint is not a generic REST API. Clients must use the MCP flow and attach a valid Fabric bearer token. |
| **Python / external applications** | Use the published data agent's MCP endpoint for runtime queries from custom apps. | The legacy Python sample uses the retired OpenAI Assistants API; don't use it as a working integration example. |
| **Native Fabric visual responses** | Let users ask for charts directly in the Fabric data agent experience. | Rendered visuals are currently available in the Fabric data agent experience, not in external clients such as SDK, Teams, Microsoft 365 Copilot, or Foundry. |

### Fabric IQ in Copilot, Excel, and Fabric Apps

The [Ignite 2026 Book of News](https://news.microsoft.com/events/ignite/2026/book-of-news/) highlights three ways to bring Fabric IQ to users beyond the Fabric portal. Choose the surface by how users work, not by whether it can host a chat.

| Surface | Best use | Architecture watch-out |
|---------|----------|------------------------|
| **Microsoft 365 Copilot Chat** | Ask questions in chat and ground answers in Power BI reports or semantic models. [Power BI data answering in Microsoft 365 Copilot Chat](https://learn.microsoft.com/en-us/fabric/iq/connectors/microsoft-365-copilot-overview) is GA. | Users need licensed access and permissions to the content; existing row-level and object-level security apply. This is distinct from [Microsoft Copilot embedded in Power BI and Fabric](https://learn.microsoft.com/en-us/fabric/iq/connectors/copilot-power-bi-fabric), which Microsoft Learn documents as preview. |
| **Excel: Edit with Copilot (preview)** | Ask questions against governed Power BI semantic models and create live, Fabric-connected PivotTables for spreadsheet analysis. | Verify preview availability and model access. Test that the workbook queries the intended model for representative users rather than treating it as a static export. |
| **Fabric Apps** | Embed conversational Q&A in an app using a Power BI semantic model for curated metrics or a published Fabric data agent for scoped analytical questions. | Verify availability and the identity used to query each source. Keep Q&A separate from write-back or workflow actions; grant and test those permissions separately. |

For GitHub Copilot ontology context and the ChatGPT and Claude semantic-model connectors, see the [Fabric IQ client previews in the MCP guide](../fabric-mcps/README.md#remote-vs-local-coverage).

## Best practices

| Practice | Why it matters |
|----------|----------------|
| **Publish only focused, domain-specific agents** | Each published data agent becomes a callable tool. Narrow tools are easier for Copilot Studio, Foundry, and MCP clients to select correctly. |
| **Write a precise data-agent description** | Tool orchestrators use the description to decide when to call the data agent. Include business domain, supported questions, source boundaries, grain, freshness, and unsupported use cases. |
| **Prefer user authentication / identity passthrough for governed data** | User-level permissions, RLS, workspace access, and source permissions should continue to shape what each user can retrieve. |
| **Validate tenant, capacity, and geography settings before exposure** | External consumption can require cross-geo AI processing/storage settings and may route responses through non-Fabric services. |
| **Keep analytical Q&A separate from operational actions** | Use the Fabric data agent for governed analytical answers; use Copilot Studio, Foundry, or another orchestration layer for actions such as approvals, ticket creation, or workflow updates. |
| **Use MCP for durable custom runtime integrations** | MCP is the published-agent runtime direction for clients that can speak the protocol and attach Fabric tokens. |
| **Test each channel with the same ground-truth questions** | A data agent that works in Fabric still needs validation in Copilot Studio, Foundry, MCP clients, and custom apps because orchestration and prompting can differ. |
| **Design visual expectations explicitly** | In native Fabric, users can ask for chart responses. In external clients, plan to reconstruct visuals from emitted specifications or use the consuming client's visualization capability. |

## Anti-patterns

| Anti-pattern | Why it fails | Do this instead |
|--------------|--------------|-----------------|
| **Publishing a broad "answer everything" data agent as a tool** | Tool orchestrators struggle to route correctly, and users get inconsistent source selection. | Publish smaller agents by business domain or outcome. |
| **Using a vague tool description** | Copilot Studio, Foundry, and MCP clients rely on the description to decide when to invoke the data agent. | Describe supported questions, source boundaries, exclusions, grain, and freshness. |
| **Choosing Maker authentication by default in Copilot Studio** | Users can receive answers through the maker's access rather than their own permissions. | Use User authentication when user-level access must persist; use Maker only with explicit approval. |
| **Treating the MCP endpoint as a simple REST endpoint** | Generic HTTP calls skip MCP initialization, tool discovery, task handling, and message format requirements. | Use an MCP-aware client with streamable HTTP and Fabric bearer-token authentication. |
| **Building new runtime apps on the old Assistants API sample path** | Microsoft Learn flags the Assistants API path as sunset and directs runtime integrations toward MCP. | Use the Python sample only as reference/migration material; build new runtime queries on MCP. |
| **Assuming every channel preserves the Fabric boundary** | Responses in Copilot Studio, Foundry, custom apps, or MCP clients can be processed/stored under that service or client's terms. | Review data handling, geography, retention, telemetry, and tenant settings for each channel. |
| **Expecting rendered visuals in every consuming client** | Rendered chart images are currently native to the Fabric data agent experience. | Treat external visual rendering as client work, or rely on the visualization specification emitted in run steps where available. |
| **Skipping republish and access validation** | Unpublished agents do not appear in Copilot Studio/Fabric IQ catalogs and MCP endpoints only work after publish. | Publish the agent, verify the catalog/MCP URL, and test access as representative users. |

## Customer validation required

- Which consumption surfaces are approved for production use.
- Whether each surface is GA or preview in the customer's tenant.
- Authentication mode and identity passthrough expectations.
- User/group access to the data agent and every underlying source.
- Tenant settings for Fabric data agents, Copilot/Azure OpenAI, cross-geo processing/storage, and external tool use.
- Data handling, retention, observability, and support ownership across Fabric and the consuming channel.

## Microsoft Learn

- [Consume a Fabric data agent as a tool in Microsoft Copilot Studio](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-microsoft-copilot-studio-tool)
- [Consume Fabric data agent from external applications with Python client SDK](https://learn.microsoft.com/en-us/fabric/data-science/consume-data-agent-python)
- [Data agent as Model Context Protocol server](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-mcp-server)
- [Earlier Microsoft Foundry integration retirement notice](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-foundry)
- [Consume Fabric data agent from Microsoft Foundry via Fabric IQ (preview)](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-foundry-fabric-iq)
- [Get visual responses from a Fabric data agent](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-visuals)
