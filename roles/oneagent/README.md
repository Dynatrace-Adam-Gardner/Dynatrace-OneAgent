# OneAgent role

## Requirements

* Supported OneAgent version >= 1.199.
* Script access to the OneAgent installer file. You can either:
  * configure the script to download the installer directly from your Dynatrace environment,
  * download it yourself and upload it to the primary node.  

### Direct download from your environment

The script utilizes [Deployment API] to download a platform-specific installer to the target machine.
You will need to supply the role with information required to authenticate the API call in your environment:

* The environment URL:
  * **SaaS**: `https://{your-environment-id}.live.dynatrace.com`
  * **Managed**: `https://{your-domain}/e/{your-environment-id}`
* The [PaaS token] of your environment

### Local installer

Use the Dynatrace UI to download OneAgent and upload it to the primary node. The script copies the installer to target machines during execution.
Note that Windows, Linux, and AIX require their dedicated installers. Original installer names indicate the target platform. If you need to change the installer names, make sure the script can distinguish them.
If you don't specify the installer, the script attempts to use the direct download.

## Variables

The following variables are available in `defaults/main/` and can be overridden:

| Name | Default | Required | Description
|-|-|:-:|-
| `oneagent_environment_url` | `-` | + | URL of the target Dynatrace environment (SaaS or Managed).
| `oneagent_paas_token` | `-` | + | The PaaS Token retrieved from the "Deploy Dynatrace" installer page.
| `oneagent_local_installer` | `-` | + | Path to local installer stored on the main node.
| `oneagent_verify_signature` | `true` | - | Enables installer's signature verification after downloading it from the environment. Not supported on Windows.
| `oneagent_installer_arch` | `-` | - | Specifies the installer architecture
| `oneagent_version` | `latest` | - | The required version of the OneAgent in 1.199.247.20200714-111723 format.
| `oneagent_download_dir` | `Linux: $TEMP or /tmp`</br>`Windows: %TEMP% or C:\Windows\Temp` | - | Installer's download directory. The directory must be available to the script. For Linux and AIX, the directory must not contain spaces.
| `oneagent_install_args` | `-` | - | Dynatrace OneAgent installation parameters defined as a list of items.
| `oneagent_platform_install_args` | `-` | - | Additional list of platform-specific installation parameters, appended to `oneagent_install_args' when run on a respective platform.
| `oneagent_preserve_installer` | `false` | - | Preserve installers on secondary machines after deployment.
| `oneagent_package_state` | `present` | - | OneAgent package state; use `present` or `latest` to make sure it's installed, or `absent` in order to uninstall.

For more information, see customize OneAgent installation documentation for [Linux], [Windows], and [AIX].

## Examples

You can find example playbooks in the `examples` directory within the role. The directory contains the following:
 -`local_installer` - basic configuration with local installers.
 -`advanced_config` - showing advanced configuration with a custom install path and download directory.

Additionally, each directory contains inventory file with basic hosts configuration for playbooks.

__NOTE:__ For multi-platform Windows, Linux or AIX deployment, you must specify the `become: true` option for proper machines group in the inventory file.
On Windows, `become: true` option is not supported.
Since Windows paths are different than a traditional Linux system, review [Path Formatting for Windows] to avoid issues during install.

[PAAS token]: https://www.dynatrace.com/support/help/shortlink/token#paas-token-
[Deployment API]: https://www.dynatrace.com/support/help/shortlink/api-deployment
[Deployment API - GET available versions of OneAgent]: https://www.dynatrace.com/support/help/shortlink/api-deployment-get-versions
[Path Formatting for Windows]: https://docs.ansible.com/ansible/latest/user_guide/windows_usage.html#path-formatting-for-windows
[Windows]: https://www.dynatrace.com/support/help/shortlink/windows-custom-installation
[Linux]: https://www.dynatrace.com/support/help/shortlink/linux-custom-installation
[AIX]: https://www.dynatrace.com/support/help/shortlink/aix-custom-installation
