.. module:: iot

***************
Ubidots Library
***************

The Zerynth Ubidots Library can be used to ease the connection to the `Ubidots IoT platform <https://ubidots.com/>`_.

It allows to make your device act as an Ubidots Device which can be created through Ubidots dashboard.

    
===============
The Device class
===============

.. class:: Device(device_label, user_type, api_token)

        Create a Device instance representing an Ubidots Device.

        The Device object will contain an mqtt client instance pointing to Ubidots MQTT broker located at :samp:`things.ubidots.com` or :samp:`industrial.api.ubidots.com` depending on :samp:`user_type`, a string which can be :samp:`"educational"` or :samp:`"business"`.
        The client is configured with :samp:`device_label` as MQTT id and is able to connect securely through TLS and to authenticate setting :samp:`api_token` as client username.

        The client is accessible through :samp:`mqtt` instance attribute and exposes all :ref:`Zerynth MQTT Client methods <lib.zerynth.mqtt>` so that it is possible, for example, to setup
        custom callback on MQTT commands (though the Device class already exposes high-level methods to setup Ubidots specific callbacks).
        The only difference concerns mqtt.connect method which does not require broker url and ssl context, taking them from Device configuration::

            my_device = iot.Device('my_label', 'business', 'my_api_token')
            my_device.mqtt.connect()
            ...
            my_device.mqtt.loop()

    
.. method:: publish(data, variable=None)

        Publish :samp:`data` dictionary to device or device variable :samp:`variable`.
        Data dictionary should follow `valid Ubidots data format <https://ubidots.com/docs/api/mqtt.html#publish>`_.

        
.. method:: on_variable_update(device, variable, callback, json=True)

        Set a callback to respond to :samp:`variable` updates from device :samp:`device`.

        :samp:`callback` will be called passing a dictionary or a float value, containing variable updates, respectively for a :samp:`True` or :samp:`False` :samp:`json` parameter value::

            def noise_callback(data):
                noise_level = data['value']
                noise_location = data['context']
                print(noise_level, noise_location)

            device.on_variable_update('noise-listener', 'noise-level', noise_callback)

        
