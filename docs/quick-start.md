# Quick Start Guide

Get up and running with Claude Code in 5 minutes using our starter templates.

## Step 1: Choose Your Project Type

**🌐 Web Application** - Building a website, web app, or SaaS product  
**🛠️ CLI Tool** - Creating a command-line utility or script  
**🔌 API/Service** - Developing a backend service or API  
**📱 Mobile App** - Building a mobile application  

## Step 2: Set Up Your Project Directory

Create a new directory for your project and navigate into it:

```bash
mkdir my-awesome-project
cd my-awesome-project
```

## Step 3: Copy the Templates

Copy all template files to your project directory:

```bash
# If you cloned this repo
cp /path/to/claude-code-starter/templates/* .

# Or copy them manually from wherever you saved them
```

You should now have these files in your project:
- `claude.md` - Master context file
- `prd.md` - Product requirements
- `infra.md` - Technical setup
- `workflow.md` - Development process
- `security.md` - Security requirements
- `sbom.md` - Dependencies
- `tests.md` - Testing strategy
- `changelog.md` - Version tracking

## Step 4: Run the Interactive Setup

Copy the content from `prompts/setup-project.prompt` and paste it into Claude Code. This will guide you through populating all the templates with your project-specific information.

Alternatively, you can use the PRD-focused prompt from `prompts/create-prd.prompt` if you want to start with just the product requirements. This step should take between 5-15 minutes, depending on how familiar you are with this process. **We highly recommend starting with the prd, and then running the Interactive Setup!**

## Step 5: Start Building

Once your templates are populated:

1. Open Claude Code
2. Make sure it can see your template files
3. Start describing what you want to build
4. Claude Code will use your templates to understand your project and build accordingly

## Example First Request

Try something like:

> "I want to create the main landing page for my app. Use the requirements from my PRD and follow the technical setup from my infrastructure file."

Claude Code will read your templates and build exactly what you need!

## Tips for Success

### ✅ Do This
- Fill out templates completely during setup
- Keep templates updated as your project evolves  (The workflow.md file contains a method to automatically do this for you) 
- Start with simple features and build up complexity
- Use the conflict resolution system in `claude.md` when templates disagree

### ❌ Avoid This
- Leaving template fields empty or with placeholder text
- Contradicting information between different templates
- Changing core architecture decisions mid-project without updating templates
- Ignoring security and testing templates for "quick" projects

## Next Steps

- Read the [Template Guide](template-guide.md) for detailed explanations
- Check out the `/examples/` folder for inspiration
- Try `/scribe` for document creation and planning workflows
- Try `/quartermaster` for technical architecture analysis
- Start building your first feature!

## Need Help?

- Review the main README.md for comprehensive information
- Look at example projects in `/examples/`
- Open an issue if you find problems
- The templates are designed to be self-documenting - read the comments!

---

**Ready to build something amazing? Lets Go!**