# Coding our mod

Now that we have a framework in place, we can start making changes to the game behaviour. Let's replicate the behaviour of our Lua mod and provide infinite oxygen, food and water to the player.

Paste this code over what's already there in `dllmain.cpp`:

```c++
#include <vector>
#include <Mod/CppUserModBase.hpp>
#include <DynamicOutput/DynamicOutput.hpp>
#include <Unreal/UObjectGlobals.hpp>
#include <Unreal/UObjectArray.hpp>
#include <Unreal/UObject.hpp>
#include <Unreal/AActor.hpp>
#include <Unreal/CoreUObject/UObject/UnrealType.hpp>

using namespace RC;
using namespace RC::Unreal;

namespace DaftAppleGames::BeginnersGuide
{
	class BeginnersGuideCppCheatMod : public CppUserModBase
	{
	public:
		// Keep a pointer to the player SurvivalAttributeSet, once found
		UObject* m_attrSet = nullptr;

	private:
		// Create a listener using NotifyUObjectCreated to wait for the player to be spawned
		// on respawned. When spawned, reset the m_attrSet pointer to null for force update to find
		// the players SurvivalAttributeSet
		struct FPlayerSpawnListener : public FUObjectCreateListener
		{
			BeginnersGuideCppCheatMod* Mod{};

			void NotifyUObjectCreated(const UObjectBase* Object, int32_t Index) override
			{
				auto* AsObject = static_cast<const UObject*>(Object);
				if (!AsObject) return;

				auto fullName = AsObject->GetFullName();
				if (fullName.find(STR("BP_Character_01_C")) != std::wstring::npos)
				{
					// Reset the cached attr set so on_update finds it fresh
					Mod->m_attrSet = nullptr;
				}
			}

			// Remove the listener if the game is shut down
			void OnUObjectArrayShutdown() override
			{
				UObjectArray::RemoveUObjectCreateListener(this);
			}
		};

		FPlayerSpawnListener m_spawnListener{};

	public:
		BeginnersGuideCppCheatMod() : CppUserModBase()
		{
			ModName = STR("BeginnersGuideCppCheatMod");
			ModVersion = STR("1.0");
			ModDescription = STR("Keeps oxygen, food and water at max values");
			ModAuthors = STR("YourName");
		}

		// Remove the listener if the mod is unloaded
		~BeginnersGuideCppCheatMod() override
		{
			UObjectArray::RemoveUObjectCreateListener(&m_spawnListener);
		}

		// When Unreal is initialised, add our listener to wait for the player to spawn
		auto on_unreal_init() -> void override
		{
			m_spawnListener.Mod = this;
			UObjectArray::AddUObjectCreateListener(&m_spawnListener);
			Log(STR("Initialised"));
		}

		// Every 'tick', we will check to see if the SurvivalAttributeSet is set, and look for it if not
		auto on_update() -> void override
		{
			// Rather than do this every frame, which is overkill and may have a performance impact, we can do it every 60 frames,
			// so about once every second if the game is running at 60FPS
			static int tickCount = 0;
			if (++tickCount < 60) return;
			tickCount = 0;

			// If we don't have a cached attr set, try to find one
			if (!m_attrSet)
			{
				// Find all UWESurvivalAttributeSet instances
				std::vector<UObject*> attrSets{};
				UObjectGlobals::FindAllOf(STR("UWESurvivalAttributeSet"), attrSets);

				// Iterate through the instances and find those belonging to our player
				for (auto* candidate : attrSets)
				{
					if (candidate)
					{
						auto fullName = candidate->GetFullName();
						if (fullName.find(STR("BP_Character_01_C")) != std::wstring::npos)
						{
							m_attrSet = candidate;
							break;
						}
					}
				}
			}

			// If we still don't have one, the player isn't spawned yet - wait silently
			if (!m_attrSet) return;

			// If we have the players attibute set, we can replenish survial attributes
			SetAttributeToMax(m_attrSet, STR("Oxygen"), STR("MaxOxygen"));
			SetAttributeToMax(m_attrSet, STR("Food"), STR("MaxFood"));
			SetAttributeToMax(m_attrSet, STR("Water"), STR("MaxWater"));
		}

	private:
		// Simple helper method to log messages prefixed with the mod name
		void Log(const std::wstring& msg)
		{
			Output::send(STR("[{}] {}\n"), ModName, msg);
		}

		// With our SurvivalAttributeSet pointer available, we can now amend the values to 'top up' the attributes to max
		void SetAttributeToMax(UObject* attrSet, const wchar_t* attrName, const wchar_t* maxAttrName)
		{
			// Read MaxOxygen.CurrentValue (or MaxFood, MaxWater etc.)
			auto maxAttrProperty = static_cast<FStructProperty*>(
				attrSet->GetPropertyByNameInChain(maxAttrName)
				);
			if (!maxAttrProperty) return;

			auto maxAttrPtr = maxAttrProperty->ContainerPtrToValuePtr<void>(attrSet);
			if (!maxAttrPtr) return;

			UStruct* attrDataStruct = maxAttrProperty->GetStruct();

			FProperty* currentValueProp = attrDataStruct->GetPropertyByNameInChain(STR("CurrentValue"));
			if (!currentValueProp) return;

			float* maxCurrentValue = currentValueProp->ContainerPtrToValuePtr<float>(maxAttrPtr);
			if (!maxCurrentValue) return;

			float maxValue = *maxCurrentValue;

			// Write attrName.BaseValue and attrName.CurrentValue
			auto attrProperty = static_cast<FStructProperty*>(
				attrSet->GetPropertyByNameInChain(attrName)
				);
			if (!attrProperty) return;

			auto attrPtr = attrProperty->ContainerPtrToValuePtr<void>(attrSet);
			if (!attrPtr) return;

			FProperty* baseValueProp = attrDataStruct->GetPropertyByNameInChain(STR("BaseValue"));
			FProperty* currentValueProp2 = attrDataStruct->GetPropertyByNameInChain(STR("CurrentValue"));

			if (baseValueProp)
			{
				float* baseValue = baseValueProp->ContainerPtrToValuePtr<float>(attrPtr);
				if (baseValue) *baseValue = maxValue;
			}
			if (currentValueProp2)
			{
				float* currentValue = currentValueProp2->ContainerPtrToValuePtr<float>(attrPtr);
				if (currentValue) *currentValue = maxValue;
			}
		}
	};
}

// Macro to simplify export methods to allow DLL methods to be called 'externally' by UE4SS
#define BEGINNERS_GUIDE_MOD_API __declspec(dllexport)
extern "C"
{
	// Allows UE4SS to load the mod
	BEGINNERS_GUIDE_MOD_API CppUserModBase* start_mod() {
		return new DaftAppleGames::BeginnersGuide::BeginnersGuideCppCheatMod();
	}

	// Allows UE4SS to unload the mod
	BEGINNERS_GUIDE_MOD_API void uninstall_mod(CppUserModBase* mod) {
		delete mod;
	}
}
```

