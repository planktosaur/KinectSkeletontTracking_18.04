include Common/CommonDefs.mak

.PHONY: all

NETPROJ = 
ifneq "$(realpath /usr/bin/gmcs)" ""
	NETPROJ += Boxes.net
endif

all: $(NETPROJ) SingleControl SceneAnalysis Players CircleControl Boxes.java Boxes PointViewer TrackPad PointServer 


.PHONY: SingleControl
SingleControl:
	$(MAKE) -C ../SingleControl

.PHONY: SceneAnalysis
SceneAnalysis:
	$(MAKE) -C ../SceneAnalysis

.PHONY: Players
Players:
	$(MAKE) -C ../Players

.PHONY: CircleControl
CircleControl:
	$(MAKE) -C ../CircleControl

.PHONY: Boxes.java
Boxes.java:
	$(MAKE) -C ../Boxes.java

.PHONY: Boxes.net
Boxes.net:
	$(MAKE) -C ../Boxes.net

.PHONY: Boxes
Boxes:
	$(MAKE) -C ../Boxes

.PHONY: PointViewer
PointViewer:
	$(MAKE) -C ../PointViewer

.PHONY: TrackPad
TrackPad:
	$(MAKE) -C ../TrackPad

.PHONY: PointServer
PointServer:
	$(MAKE) -C ../PointServer
