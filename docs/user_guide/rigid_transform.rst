Rigid body transformation of RAOs
=================================

Rigid body transformation of surge, sway and heave RAOs is governed by the following
equations:

.. math::

    H_{x_j}(\omega) = H_{x_i}(\omega) - y_{ij}H_{\gamma}(\omega) + z_{ij} H_{\beta}(\omega)

.. math::

    H_{y_j}(\omega) = H_{y_i}(\omega) + x_{ij}H_{\gamma}(\omega) - z_{ij}H_{\alpha}(\omega)

.. math::
    H_{z_j}(\omega) = H_{z_i}(\omega) - x_{ij}H_{\beta}(\omega) + y_{ij}H_{\alpha}(\omega)

where :math:`x_{ij}`, :math:`y_{ij}` and :math:`z_{ij}` are the coordinates of a 'new' location
(*j*), relative to an 'old' location (*i*). :math:`H_x(\omega)` is the surge RAO,
:math:`H_y(\omega)` is the sway RAO, :math:`H_z(\omega)` is the heave RAO,
:math:`H_{\alpha}(\omega)` is the roll RAO, :math:`H_{\beta}(\omega)` is the pitch RAO,
and :math:`H_{\gamma}(\omega)` is the yaw RAO.

.. note::

    Only the translational degrees-of-freedom (i.e., surge, sway and heave)
    need to be transformed in order to obtain RAOs for a different location
    on a rigid body. The rotational motions (i.e., roll, pitch and yaw) are independent
    of location, and will be the same for all points on a rigid body.

With ``waveresponse`` you can easily transform RAOs from one location to another
on a rigid body using the :meth:`~waveresponse.rigid_transform` function. You must
then provide a 'translation vector', `t`, that determines the coordinates of the new
location, *j*, relative to the old location, *i*.

.. code-block:: python

    import numpy as np
    import waveresponse as wr


    # Translation vector
    t = np.array([10.0, 0.0, 0.0])   # (x, y, z) coordinates of j relative to i

    # Rigid body transform surge, sway and heave RAOs
    surge_j, sway_j, heave_j = wr.rigid_transform(t, surge_i, sway_i, heave_i, roll, pitch, yaw)

Alternatively, you can transform the degrees-of-freedom one at a time:

.. code-block:: python

    # Rigid body transform surge RAO only
    surge_j = wr.rigid_transform_surge(t, surge_i, pitch, yaw)

    # Rigid body transform sway RAO only
    sway_j = wr.rigid_transform_sway(t, sway_i, roll, yaw)

    # Rigid body transform heave RAO only
    heave_j = wr.rigid_transform_heave(t, heave_i, roll, pitch)

.. tip::

    The rigid body transformations provided by ``waveresponse`` are only valid for
    'displacement' RAOs. If you want to obtain 'velocity' or 'acceleration' RAOs
    for a new location, you can achieve that by first transforming the displacement
    RAOs, and then differentiate the new :class:`~waveresponse.RAO` objects by calling
    :meth:`~waveresponse.RAO.differentiate`.