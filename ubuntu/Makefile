PACKER ?= packer

.PHONY: all clean

all: custom-ubuntu-lvm.dd.gz

seeds-lvm.iso: https-server/ubuntu-2004-amd64-qemu/user-data meta-data
	cloud-localds $@ $^

seeds-flat.iso: http-server/ubuntu-2004-amd64-qemu/user-data http-server/ubuntu-2004-amd64-qemu/meta-data
	cloud-localds $@ $^

OVMF_VARS.fd: /usr/share/OVMF/OVMF_VARS.fd
	cp -v $< $@

custom-ubuntu.tar.gz: clean seeds-flat.iso OVMF_VARS.fd \
			provisioning/packages/custom-packages.tar.gz
	sudo PACKER_LOG=1 ${PACKER} build ubuntu-flat.json && reset

custom-ubuntu-lvm.dd.gz: clean seeds-lvm.iso OVMF_VARS.fd
	sudo PACKER_LOG=1 ${PACKER} build ubuntu-2004-amd64-qemu.pkr.hcl && reset

clean:
	sudo ${RM} -rf output-qemu custom-ubuntu*.gz

CUSTOM_PKGS:=${wildcard provisioning/packages/*.deb}

provisioning/packages/custom-packages.tar.gz: ${CUSTOM_PKGS}
	tar czf $@ -C provisioning/packages ${notdir $^}

.INTERMEDIATE: OVMF_VARS.fd
