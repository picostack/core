# Grafana

This directory contains dashboards that are mounted into the Grafana container.

You can use dashboard IDs from the Grafana dashboard marketplace but this method keeps them in source control and allows you to make tweaks that suit your situation or keep them up to date.

There's also a plugin in the `plugins/` directory that's added as a Git submodule so it can be updated easily.

## Editing Dashboards

To edit a dashboard while keeping it in sync, you can use the Grafana UI to make your changes, then click the "Save" button at the top right. Since Grafana is aware of dashboards that are added via the filesystem (known as "provisioned" dashboards) it will let you know you can't save the changes there and gives you the raw JSON data.

Then, all you need to do is store your changes in your repo. Or, if you're pulling directly from _this_ repo, open a pull request and contribute your changes back to the community!
