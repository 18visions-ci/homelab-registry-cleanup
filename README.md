# homelab-registry-cleanup

This repository contains an Ansible playbook to automate the garbage collection process for a Docker registry and send a notification with the output to a Discord channel.

## Prerequisites

Before running this playbook, ensure you have the following:

*   **Ansible**: Installed on your control machine ( I used Semaphore UI with a template schedule).
*   **Target Node**: A host defined as `registry-node` in your Ansible inventory that is running the Docker registry.
*   **Garbage Collection Script**: The script `/usr/local/bin/prune_docker_registry.sh` must be present and executable on the target node.
*   **Sudo Access**: The Ansible user must have passwordless `sudo` privileges on the target node.
*   **Discord Webhook**: A valid Discord webhook URL.

## Usage

1.  Clone the repository to your local machine.
2.  Run the playbook using `ansible-playbook`, providing your inventory file and the Discord webhook URL as an extra variable.

    ```sh
    ansible-playbook playbook.yaml -i <path/to/your/inventory> --extra-vars "discord_webhook_url=YOUR_DISCORD_WEBHOOK_URL"
    ```

## Playbook Details

The [`playbook.yaml`](playbook.yaml) performs the following tasks:

1.  **Run Garbage Collection**: Executes the `/usr/local/bin/prune_docker_registry.sh` script on the `registry-node` with `root` privileges.
2.  **Format Discord Message**: Captures the output of the script and formats it for a Discord message.
3.  **Send Notification**: Posts the message to the provided Discord webhook URL.
