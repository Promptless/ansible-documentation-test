I'm sorry, I need the specific context of the code changes and the current document content to provide an updated version of the documentation. Could you please provide those details?

#. If you are removing an action plugin, remove the corresponding module that contains the documentation.
#. If you are removing a module, remove any corresponding action plugin that should stay with it.
#. Remove any entries about removed plugins from ``meta/runtime.yml``. Ensure they are added into the new repo.
#. Remove sanity ignore lines from ``tests/sanity/ignore\*.txt``
#. Remove associated integration tests from ``tests/integrations/targets/`` and unit tests from ``tests/units/plugins/``.
#. if you are removing from content from ``community.general`` or ``community.network``, remove entries from ``.github/BOTMETA.yml``.
#. Carefully review ``meta/runtime.yml`` for any entries you may need to remove or update, in particular deprecated entries.
#. Update ``meta/runtime.yml`` to contain redirects for EVERY PLUGIN, pointing to the new collection name.

.. warning::

	Maintainers for the old collection have to make sure that the PR is merged in a way that it does not break user experience and semantic versioning:

	#. A new version containing the merged PR must not be released before the collection the content has been moved to has been released again, with that content contained in it. Otherwise the redirects cannot work and users relying on that content will experience breakage.
	#. Once 1.0.0 of the collection from which the content has been removed has been released, such PRs can only be merged for a new **major** version (in other words, 2.0.0, 3.0.0, and so on).


Updating BOTMETA.yml
--------------------

The ``BOTMETA.yml``, for example in `community.general collection repository <https://github.com/ansible-collections/community.general/blob/main/.github/BOTMETA.yml>`_, is the source of truth for:

* ansibullbot

If the old and/or new collection has ``ansibullbot``, its ``BOTMETA.yml`` must be updated correspondingly.

Ansibulbot will know how to redirect existing issues and PRs to the new repo. The build process for docs.ansible.com will know where to find the module docs.

.. code-block:: yaml

   $modules/monitoring/grafana/grafana_plugin.py:
       migrated_to: community.grafana
   $modules/monitoring/grafana/grafana_dashboard.py:
       migrated_to: community.grafana
   $modules/monitoring/grafana/grafana_datasource.py:
       migrated_to: community.grafana
   $plugins/callback/grafana_annotations.py:
       maintainers: $team_grafana
       labels: monitoring grafana
       migrated_to: community.grafana
   $plugins/doc_fragments/grafana.py:
       maintainers: $team_grafana
       labels: monitoring grafana
       migrated_to: community.grafana

`Example PR <https://github.com/ansible/ansible/pull/66981/files>`_

* The ``migrated_to:`` key must be added explicitly for every *file*. You cannot add ``migrated_to`` at the directory level. This is to allow module and plugin webdocs to be redirected to the new collection docs.
* ``migrated_to:`` MUST be added for every:

  * module
  * plugin
  * module_utils
  * contrib/inventory script

* You do NOT need to add ``migrated_to`` for:

  * Unit tests
  * Integration tests
  * ReStructured Text docs (anything under ``docs/docsite/rst/``)
  * Files that never existed in ``ansible/ansible:devel``

.. seealso::

   :ref:`collections`
       Learn how to install and use collections.
   :ref:`contributing_maintained_collections`
       Guidelines for contributing to selected collections
   :ref:`Communication<communication>`
       Got questions? Need help? Want to share your ideas? Visit the Ansible communication guide
