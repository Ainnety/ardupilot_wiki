.. _common-do-land-start:
[copywiki destination="copter,plane,sub,rover,planner"]
===================
Using DO_LAND_START
===================

The :ref:`DO_LAND_START <mav_cmd_do_land_start>` mission item can be used for two purposes:

#. In Plane, to provide an automatic re-attempt for an aborted NAV_LAND autolanding in a mission. (See :ref:`aborting-autolanding`)
#. To provide an automatic landing sequence for RTLs in Plane or as a failsafe action in Copter.

.. note:: in Rover, it can be used, but only via Scripting call to start the landing sequence.

[site wiki="plane"]
To trigger an automatic landing as part of an RTL(return to launch) you need to do two things:

-  add a :ref:`DO_LAND_START <mav_cmd_do_land_start>` mission item to your mission, just before the start of your landing sequence which sets up the approach waypoints and ends with a NAV_LAND item.
-  set the :ref:`RTL_AUTOLAND <RTL_AUTOLAND>` parameter to 1 or 2

The exact behaviour depends on the :ref:`RTL_AUTOLAND<RTL_AUTOLAND>` value:
-  If :ref:`RTL_AUTOLAND<RTL_AUTOLAND>` = 1, then the plane will first RTL as normal, then when it starts circling the return point (home or a rally point) it will then switch to the AUTO mission after the ``DO_LAND_START`` 
-  If :ref:`RTL_AUTOLAND<RTL_AUTOLAND>` = 2, then the plane will bypass the RTL completely and go straight to the landing sequence after the ``DO_LAND_START``.

.. note:: the "landing sequence" whose start is the ``DO_LAND_START`` marker, does not actually have to include a landing! it can be any sequence of valid mission commands. For example, flying to a location, and changing to an indefinite VTOL loiter (applies only to a QuadPlane, obviously).

.. note:: ArduPilot checks to see if there is a ``DO_LAND_START`` mission items before arming, and :ref:`RTL_AUTOLAND<RTL_AUTOLAND>` is set to 0. If so, a pre-arm failure condition will prevent arming. If it is desired to use a ``DO_LAND_START`` only for aborted autolandings and not as an RTL action override, then set :ref:`RTL_AUTOLAND<RTL_AUTOLAND>` = 3, to allow arming. Its use for an aborted autolanding is explained in :ref:`aborting-autolanding`.

.. note:: if you execute an RTL due to a battery failsafe and the :ref:`MIS_OPTIONS<MIS_OPTIONS>` bit 1 (Use distance to land) is set, then if continuing the mission results in a closer landing point, then the mission will proceed.

[/site]
[site wiki="copter"]

To use, you need to do two things:

-  add a :ref:`DO_LAND_START <mav_cmd_do_land_start>` mission item to your mission, just before the start of your landing sequence(s).
-  use ``DO_LAND_START`` as a failsafe action. For example, setting :ref:`FS_THR_ENABLE<FS_THR_ENABLE>` = "6" for RC failsafe.

.. note:: the "landing sequence" whose start is the ``DO_LAND_START`` marker, does not actually have to include a landing! it can be any sequence of valid mission commands.
[/site]

You can optionally include more than one ``DO_LAND_START`` mission item in your mission. If that is done then the latitude/longitude/altitude of the ``DO_LAND_START`` mission items is used to choose which landing sequence to use. The ``DO_LAND_START`` closest to the current location in all three dimensions is used. This can be useful if you have multiple landing sequences for different wind conditions or different areas.

.. note:: If you are already in a landing sequence (NAV_LAND or DO_START_LAND sequence), then it will continue.
