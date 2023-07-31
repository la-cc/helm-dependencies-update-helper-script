# Helm Dependencies Update Helper (Script) - WIP!!!

This is a script to help you to update your helm dependencies in your helm charts. It will create a PR with the changes if you want to. But

Why I build this tool? We have a lot of helm charts and we want to update the dependencies in all of them.
The resources from helm-charts will be deployed over argocd, so the helm chart will no be installed on the cluster itself.
Because of that you cant use tools like [renovate](https://github.com/renovatebot/helm-charts) or [dependabot](https://github.com/dependabot).

You can use this tool in your CI/CD pipeline or locally. The whole description about the script can be found on TBD.

## Requirements

You will need the following tools installed on your machine (depends on your target environment):

- [yq](https://github.com/mikefarah/yq)
- [dyff](https://github.com/homeport/dyff)
- [az cli](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)
- [azure-devops cli](https://learn.microsoft.com/en-us/azure/devops/cli/?view=azure-devops)
- [gh cli](https://cli.github.com/manual/installation)

Please feel to fork this repository. This allows you to customize the script for your needs. Moreover you test the script directly in your environment.

## Dry-Run

The default for the `config.env` file looks like:

    BRANCH='main'
    DRY_RUN='true'
    GITHUB='false'
    AZURE_DEVOPS='false'
    WITHOUT_PR='false'

This will only print the changes to the console. You can run the script with:

    ./check-helm-dep-updates.sh

You will get an output like:

    ####################### Begin #######################
    Name: External DNS
    Version in Chart.yaml: 6.20.4
    Current Version: 6.21.0
    There's a difference between the versions.
    Differences:
        _        __  __
    _| |_   _ / _|/ _|  between old_values.yaml
    / _' | | | | |_| |_       and new_values.yaml
    | (_| | |_| |  _|  _|
    \__,_|\__, |_| |_|   returned two differences
            |___/

    (root level)
    + one map entry added:
    ## @param ingressClassFilters Filter sources managed by external-dns via IngressClass (optional)
    ##
    ingressClassFilters: []

    image.tag
    ± value change
        - 0.13.4-debian-11-r19
        + 0.13.5-debian-11-r55

    ####################### End #######################

## GitHub

First you have to login against GitHub. You can do this with `gh auth login` or get more information here on the official [documentation](https://cli.github.com/manual/gh_auth_login)

Now edit the `config.env` file like:

    BRANCH='main'
    DRY_RUN='false'
    GITHUB='true'
    AZURE_DEVOPS='false'
    WITHOUT_PR='false'

Commit and push the changes to your repository. After that you can run the script with:

**WARNING:** after you set GITHUB to true, the script will create a new branch and a PR with the changes. If you want to test it first, set DRY_RUN to true.

Execute the script with:

    ./check-helm-dep-updates.sh

Check the changes on GitHub and if you are happy with them, you can merge the PR.

## Azure DevOps

First you have to create a PAT token. You can find more information here on the official [documentation](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=azure-devops&tabs=preview-page). After you created the PAT token, you need to set the following variable:

    AZURE_DEVOPS_EXT_PAT=<your-pat-token>

Then edit the `config.env` file like:

    BRANCH='main'
    DRY_RUN='false'
    GITHUB='false'
    AZURE_DEVOPS='true'
    WITHOUT_PR='false'

**WARNING:** after you set AZURE_DEVOPS to true, the script will create a new branch and a PR with the changes. If you want to test it first, set DRY_RUN to true.

Execute the script with:

    ./check-helm-dep-updates.sh

Check the changes on Azure DevOps and if you are happy with them, you can merge the PR.

## Without PR - Support all Git Providers

This part allows you to update the helm dependencies without creating a PR. You can use this part in your CI/CD pipeline.
It can be useful if you want to update the helm dependencies in your main branch over an manual pull request.

First edit the `config.env` file like:

    BRANCH='main'
    DRY_RUN='false'
    GITHUB='false'
    AZURE_DEVOPS='false'
    WITHOUT_PR='true'

**WARNING:** after you set WITHOUT_PR to true, the script will create a new branch and push this branch. If you want to test it first, set DRY_RUN to true.

Execute the script with:

    ./check-helm-dep-updates.sh

Check the changes on your Git Provider and if you are happy with them, you can merge the PR.

## The CI/CD Way

### Azure DevOps Pipelines

This part is still WIP. I will update this part as soon as possible. You can already use the script in your CI/CD pipeline.
I want to create a GitHub Action for this script.

### GitHub Actions - TBD

This part is still WIP. I will update this part as soon as possible. You can already use the script in your CI/CD pipeline.
I want to create a GitHub Action for this script.
