.PHONY: all

ROOT = $(dir $(realpath $(firstword $(MAKEFILE_LIST))))

all: workflows/nightly.yml workflows/release.yml workflows/tests.yml \
    workflows/tests-managed-pg.yml workflows/tests-ha.yml \
    workflows/tests-pg-versions.yml workflows/tests-patches.yml

workflows/%.yml: workflows.src/%.tpl.yml workflows.src/%.targets.yml workflows.src/tests.inc.yml
	$(ROOT)/workflows.src/render.py $* $*.targets.yml

workflows.src/tests.tpl.yml: workflows.src/tests.inc.yml
	touch $(ROOT)/workflows.src/tests.tpl.yml

workflows.src/tests-managed-pg.tpl.yml: workflows.src/tests.inc.yml
	touch $(ROOT)/workflows.src/tests-managed-pg.tpl.yml

workflows.src/tests-ha.tpl.yml: workflows.src/tests.inc.yml
	touch $(ROOT)/workflows.src/tests-ha.tpl.yml

workflows.src/tests-pg-versions.tpl.yml: workflows.src/tests.inc.yml
	touch $(ROOT)/workflows.src/tests-pg-versions.tpl.yml
