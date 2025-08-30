# Chems-Unused-Stuff
A collection of Chemthunder's unused code for minecraft mods, mainly so ArcCM can stop ripping me off directly


// Exp Sacrifice Mechanic (aka Soulfork from Impaled)
Quite simple, upon right-click detects if the user has an experience level above zero, the proceeds to run an action.
In this instance, it will dash the player forward

this code runs on 1.21.8, so may need some skill to backport.

@Override
    public ActionResult use(World world, PlayerEntity user, Hand hand) {
        ItemStack stack = user.getMainHandStack();
        if (world instanceof ServerWorld serverWorld) {
            if (user.experienceLevel > 0) {
                user.addExperienceLevels(-1); // removes a level

                user.addVelocity(user.getRotationVec(0).multiply(5)); // launches the player forward in the direction they are facing
                user.velocityModified = true;

                user.getItemCooldownManager().set(stack, 30); // cooldown

                user.playSound(NorthSounds.SWOOSH); //sound

            } else {

                user.addVelocity(user.getRotationVec(0).multiply(5));
                user.velocityModified = true;
                user.damage(serverWorld, NorthDamageTypes.throngled(user), 4f); // if the player doesn't have an exp level, does damage to the user instead

            }
            return super.use(world, user, hand);
        }
        return null;
    }