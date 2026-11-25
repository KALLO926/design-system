# Self hosted fonts for China facing pages

## Find every external font request

- Search the source and the built output for fonts.googleapis.com and fonts.gstatic.com before you deliver anything the user will open in China.

- Treat icon fonts and design fonts the same way as body fonts and search for every remote font link, remote font import and remote font face rule.

- List each external font request you find with the file that loads it and do not deliver while any request remains.

## Bundle and self host the fonts

- Bundle each needed font face locally beside the product and serve it from the same origin as the page.

- Bundle the complete Simplified Chinese face for any product that shows Chinese text and include both regular and bold weights when the product uses both.

- Subset the bundled font to the characters the product actually shows and keep the subset complete for every real string in the source.

- Remove the remote font link after the local copy works and do not keep it as a fallback.

## Verify zero external requests before delivery

- Open the delivered page with the network blocked for font hosts and confirm every character renders in the intended glyphs.

- Check every real text string in the product source character by character against the bundled subset and expand the subset when any character is missing.

- Confirm the page makes zero external requests on open and say so in the delivery note with how you checked.

- Ask the user to judge the result on their own screen when your review environment cannot load the fonts the way their device does and do not claim the rendering is verified when it is not.

## Available connectors

- Use `muse.connector_search` to find other connectors and `muse.skill_search` to find related skills.

- If nothing here fits, look on the web for a maintained API or MCP server and set it up with [Custom Connectors and MCPs](/opt/muse/skills/custom-connector-setup/SKILL.md), or build a custom connector.

## Alternatives for better results

- Run the font metrics skill after self hosting when text still needs vertical alignment checks.

- Limit the bundled weights to the ones the product actually uses when the bundle grows too large for the delivery format.

- Prefer a plain system font stack only for a page that shows Latin text alone and has no Chinese text to garble.
