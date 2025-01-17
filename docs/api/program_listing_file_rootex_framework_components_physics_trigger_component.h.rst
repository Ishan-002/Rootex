
.. _program_listing_file_rootex_framework_components_physics_trigger_component.h:

Program Listing for File trigger_component.h
============================================

|exhale_lsh| :ref:`Return to documentation for file <file_rootex_framework_components_physics_trigger_component.h>` (``rootex\framework\components\physics\trigger_component.h``)

.. |exhale_lsh| unicode:: U+021B0 .. UPWARDS ARROW WITH TIP LEFTWARDS

.. code-block:: cpp

   #pragma once
   
   #include "common/common.h"
   #include "scene.h"
   #include "collision_component.h"
   #include "components/space/transform_component.h"
   
   #include "BulletCollision/CollisionDispatch/btGhostObject.h"
   
   class TriggerComponent : public CollisionComponent
   {
       COMPONENT(TriggerComponent, Category::Physics);
       DEPENDS_ON(TransformComponent);
   
       Ref<btBoxShape> m_BoxShape;
       btGhostObject* m_Body = nullptr;
   
       Vector3 m_Dimensions;
       bool m_EntryRepeat;
       Vector<SceneID> m_EntryTargetIDs;
   
       bool m_ExitRepeat;
       Vector<SceneID> m_ExitTargetIDs;
   
       Vector<SceneID> m_EntryList;
       Vector<SceneID> m_LastUpdateEntryList;
       bool m_IsEntryNotified = false;
       bool m_IsExitNotified = false;
   
       void openRegister();
       void registerEntry(SceneID id);
       int findExitCount() const;
       void closeRegister();
   
       void notifyEntry();
       void notifyExit();
       bool isExitNotified() { return m_IsExitNotified; }
       bool isEntryNotified() { return m_IsEntryNotified; }
       bool canNotifyEntry() { return !isEntryNotified() || isEntryRepeat(); }
       bool canNotifyExit() { return !isExitNotified() || isExitRepeat(); }
   
       void addTarget(Vector<SceneID>& list, SceneID toAdd);
       void removeTarget(Vector<SceneID>& list, SceneID toRemove);
   
       void updateTransform();
   
       friend class TriggerSystem;
   
   public:
       TriggerComponent(Entity& owner, const JSON::json& data);
       ~TriggerComponent() = default;
   
       bool isEntryRepeat() { return m_EntryRepeat; }
       bool isExitRepeat() { return m_ExitRepeat; }
   
       void setDimensions(const Vector3& dimensions);
   
       void addEntryTarget(SceneID toAdd);
       void addExitTarget(SceneID toAdd);
       void removeEntryTarget(SceneID toRemove);
       void removeExitTarget(SceneID toRemove);
   
       btGhostObject* getGhostObject() { return m_Body; };
   
       bool setupData() override;
       JSON::json getJSON() const override;
       void draw() override;
   };
   
   DECLARE_COMPONENT(TriggerComponent);
