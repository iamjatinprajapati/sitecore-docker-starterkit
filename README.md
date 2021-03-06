# yask for Sitecore Docker
_yet-another-starterkit_ for Sitecore

This starter-kit generates a preconfigured local docker environment based on selected Sitecore topology, variant and module(s).

It uses a "layering" technique to build up a folder structure by following a concept similar to how Docker images are built.

**This is work-in-progress**  
_a ton of clean-up and refactoring is coming. this is currently a copy of a quick-n-dirty boilerplate from the Sitecore Hackathon_

The concept and initial commit originate from a quick boilerplate made for the Sitecore Hackathon participants to use in the Sitecore 2021 Hackathon. 

The goal now is to extract a clean Powershell module that fetch files from a separate repo. Solution and msbuild setup will be removed from the starter-kit so it is only used to generate the docker environment files based on the selected Sitecore topology, variant and optional modules.

## Basic concept

The sitecore topology, variant and modules are selected through a fancy colorful console that builds up a string similar, but not identical, to sitecore image tags.

F. ex. sitecore-xp0-jss

This will copy in the folders in the given order
1. sitecore
2. sitecore-xp0
3. sitecore-xp0-jss

sitecore being the base setup. Each higher level folder only contain the delta between the desired topology/variant and the base.

After the folders has been merged into a new .\docker folder the fancy console ui prompts for all required environment values that is then stored in the .env file 

When the wizard is done, the docker environment is ready to spin up with no further changes needed. Deployment from Visual Studio to shared volume folders etc. can be bootstrapped from other templates, such as [VS Helix templates extension](https://github.com/LaubPlusCo/Helix-Templates) or other.

_note; this starter-kit may or may not evolve any further. Please help by contributing if you like the concept_ 

# Usage instructions
## Prerequisites 
Refer to the prerequisites from the [Sitecore Getting Started template walk-through](https://doc.sitecore.com/developers/100/developer-tools/en/walkthrough--using-the-getting-started-template.html). 

It is recommended to have followed this walk-through succesfully before making use of this Starter-kit to ensure all prerequisites are in place.

## Initial setup
> If you have IIS running then stop it by running `iisreset /stop` or open 'IIS manager' > _right click computername_ > 'Stop'

1. Open a powershell terminal as Administrator
2. Navigate to this folder and run `.\Start-Hackathon.ps1`
3. Follow on-screen instructions
4. Test, git commit and push so other team members can pull the generated setup
5. Other team members should then git pull and run `.\Start-Hackathon.ps1` to generate locally trusted certificates and add hosts entries

If setup fails or you need to re-run the setup wizard for other reasons then run `.\Remove-Starterkit.ps1` to reset and run `.\Start-Hackathon.ps1` again.

After the initial setup `.\Start-Hackathon.ps1` can be used to bring up the containers - similar to manually running `.\docker-compose up -d` from the `.\docker` folder.

To take the containers down again you can use `.\Stop-Hackathon.ps1` - this calls `docker-compose down` followed by `docker system prune`. This can sometime help if you experience issues with the Sitecore containers not starting up or do not respond after computer has been turned off or put in Sleep-mode.

If the certificates are invalid then make sure that the mkcert self-signed root certificate is in `Local Machine > Trusted root certificates.`

## Deviations from Sitecore Docker examples

_docker-compose files has been moved the `.\docker` folder to keep the solution root clean._

_all images has Sitecore Powershell Extensions pre-installed_  

# Some credits and references

- [Original Hackathon boilerplate repo](https://github.com/Sitecore-Hackathon/Boilerplate/tree/feature/2021)
- [Sitecore Community docker images](https://github.com/Sitecore/docker-images)
- [Sitecore Docker tools](https://github.com/Sitecore/docker-tools)
- Some inspiration [Konabos docker-examples](https://github.com/konabos/docker-examples)
