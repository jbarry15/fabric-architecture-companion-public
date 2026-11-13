# Contributing

Thank you for helping improve this Fabric architecture companion. This repository is intended to contain reusable, customer-safe guidance, templates, reference patterns, and notebook/tooling examples for Microsoft Fabric architecture.

## Contributor License Agreement

Most contributions require you to agree to a Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us the rights to use your contribution. For details, visit the [Contributor License Agreements portal](https://cla.opensource.microsoft.com).

When you submit a pull request, a CLA bot can automatically determine whether you need to provide a CLA and decorate the PR appropriately with a label or comment. Follow the instructions provided by the bot. You only need to do this once across repositories that use the Microsoft CLA.

## Code of Conduct

This project follows the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information, see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with questions or comments.

## What to contribute

Good contributions include:

- Corrections to Fabric architecture guidance.
- Public Microsoft Learn links that clarify product behavior.
- Reusable best practices, anti-patterns, reference architectures, or examples with no confidential details.
- Customer-safe templates, checklists, and examples.
- Improvements to notebooks or tools that do not require private data or internal services.

## Do not contribute confidential information

Do **not** include:

- Customer names, account names, TPIDs, tenant IDs, workspace IDs, subscription IDs, or environment-specific identifiers.
- Secrets, tokens, keys, connection strings, passwords, or private endpoints.
- Screenshots that show names, emails, tenant URLs, workspace names, internal systems, or sensitive data.
- Microsoft-internal links, internal roadmap details, field-only material, or non-public announcements.
- Customer data, proprietary architecture diagrams, or engagement notes unless they are fully anonymized and approved for sharing.

If a scenario comes from real field work, convert it into a reusable pattern and remove identifying details before opening a pull request.

## Contribution process

1. Open an issue or pull request with a clear description of the change.
2. Keep changes focused. Avoid unrelated formatting or broad rewrites.
3. Prefer Microsoft Learn links for current product behavior, requirements, limits, and setup steps.
4. If repo guidance and Microsoft Learn differ, follow Microsoft Learn and call out the local page that may need updating.
5. For notebooks or scripts, avoid checked-in outputs that contain environment, tenant, customer, or execution data.
6. Follow the [Solutions placement guidance](solutions/README.md#adding-a-solution): put external-repository reference cards under the workload's `repos/` folder and keep original scenario and decision guides at the workload root.

## AI-assisted contributions

AI-assisted contributions are welcome when they are transparent, reviewed, and tested by the contributor. If AI helped create the contribution, disclose the tools used and how they influenced the work in the pull request or issue summary.

When submitting AI-assisted changes:

- Understand the content you are contributing and be ready to answer follow-up questions.
- Verify the changes against public Microsoft Learn documentation where product behavior is discussed.
- Avoid bulk generated edits that do not address a specific repository need.
- Include evidence such as screenshots, link checks, or test output when the change requires validation.

## Pull request checklist

Before submitting, confirm:

- [ ] The content is customer-safe and approved to share.
- [ ] No secrets, IDs, customer names, internal URLs, or private screenshots are included.
- [ ] Product behavior is backed by current Microsoft Learn documentation where appropriate.
- [ ] Links are public and work without internal Microsoft access.
- [ ] The change is scoped to the requested topic.

## Review expectations

Maintainers may ask for edits to improve clarity, remove sensitive details, align with repository patterns, or verify product behavior against Microsoft Learn.
