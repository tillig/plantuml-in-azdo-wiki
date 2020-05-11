# PlantUML in Azure DevOps Wiki

This repo shows one way to put PlantUML diagrams in Azure DevOps wiki.

It's primarily useful in wikis that are [published from Git repositories](https://docs.microsoft.com/en-us/azure/devops/project/wiki/publish-repo-to-wiki?view=azure-devops&tabs=browser); standard wikis won't work with this because you won't have access to add files in the same way.

The process is:

- Run `npm install` to restore all the required Node.js packages.
- Run `npm run watch`. This starts up `index.js` which is a filesystem watcher tied to a PlantUML image generator.
- Create PlantUML documents using a `.puml` extension.
- As `.puml` files are added, changed, and removed, `.png` image versions of the diagrams will be created based on those files. Each `.png` will have the same base filename as the `.puml` file.
- Include the Markdown to show the image in your wiki page.

If you're in VS Code, there's already a `watch` task wired up and ready to go. Start that, edit your wiki, and things will just work in the background.

**The `Diagrams` folder shows a simple wiki page with a PlantUML diagram embedded.**

Other interesting bits:

- The `.plantstyles` file is tied into the image generation in `index.js` - you can change the style for images using that file and images will be properly formatted.
- `.editorconfig` and `.markdownlint.json` are wired up to help the wiki markdown stay consistent. These will work with VS Code and the [markdownlint extension](https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint) to help you keep things clean. However, this isn't required for the PlantUML integration.

It's not as automatic as [the Mermaid support](https://docs.microsoft.com/en-us/azure/devops/project/wiki/wiki-markdown-guidance?view=azure-devops#add-mermaid-diagrams-to-a-wiki-page), but since [Mermaid is really slow to update in Azure DevOps](https://developercommunity.visualstudio.com/idea/883356/mermaid-support-for-class-diagrams-and-state-diagr.html) and [there's no traction on getting PlantUML into Azure DevOps](https://developercommunity.visualstudio.com/idea/747577/add-support-for-plantuml.html), it gets the job done.

I didn't implement this as a CI build because it would mean the build itself needs to commit the generated images back to the original repo and I didn't want a build doing that. You could make it more complex and store the images in a separate repo, but then the wiki isn't as self-contained. This local-build integration seemed a reasonable compromise.
