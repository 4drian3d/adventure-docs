======
Bukkit
======

The Adventure platform implementation for Bukkit targets Paper, Spigot, and Bukkit for
Minecraft 1.7.10 through 1.20.1.

.. warning::

  This page documents only the *legacy* platform adapter for Bukkit. Most users should use `Paper <https://papermc.io>`_'s native implementation instead.
  This native implementation provides more functionality, better integration with the server, and does not require the :java:`BukkitAudiences` adapter.

.. kyori-dep:: adventure-platform-bukkit platform

Usage
-----

You should first obtain a :java:`BukkitAudiences` object by using :java:`BukkitAudiences.create(plugin)`. This object is thread-safe
and can be reused from different threads if needed. From here, Bukkit ``CommandSender`` s and ``Player`` s may be converted into
``Audience`` s using the appropriate methods on ``BukkitAudiences``.

The audiences object should also be closed when a plugin is disabled in order to clean up resources and increase the likelihood of a successful ``/reload``.

.. code:: java

   public class MyPlugin extends JavaPlugin {

     private BukkitAudiences adventure;

     public @NonNull BukkitAudiences adventure() {
       if(this.adventure == null) {
         throw new IllegalStateException("Tried to access Adventure when the plugin was disabled!");
       }
       return this.adventure;
     }

     @Override
     public void onEnable() {
       // Initialize an audiences instance for the plugin
       this.adventure = BukkitAudiences.create(this);
       // then do any other initialization
     }

     @Override
     public void onDisable() {
       if(this.adventure != null) {
         this.adventure.close();
         this.adventure = null;
       }
     }
   }

This audience provider should be used over the serializers directly, since it will handle compatibility measures for sending messages across versions.


Component serializers
---------------------

For areas that aren't covered by the ``Audience`` interface, the Bukkit platform provides the ``MinecraftComponentSerializer`` (available on CraftBukkit-based servers), and the ``BungeeComponentSerializer`` (available on Spigot and Paper servers) to convert directly between Adventure :doc:`Components </text>` and other component types. For uses that don't integrate directly with native types, JSON and legacy format serializers for the running server version are exposed in ``BukkitComponentSerializer``.
