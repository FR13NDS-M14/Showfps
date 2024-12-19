using StardewModdingAPI;
using StardewModdingAPI.Events;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace ShowFPS
{
    public class ModEntry : Mod
    {
        private int frameCount = 0;
        private double elapsedTime = 0;
        private int currentFPS = 0;

        public override void Entry(IModHelper helper)
        {
            GraphicsEvents.OnPostRenderHudEvent += OnPostRenderHud;
            GameEvents.UpdateTick += OnUpdateTick;
        }

        private void OnUpdateTick(object sender, EventArgs e)
        {
            elapsedTime += Game1.currentGameTime.ElapsedGameTime.TotalMilliseconds;

            if (elapsedTime >= 1000)
            {
                currentFPS = frameCount;
                frameCount = 0;
                elapsedTime = 0;
            }
            else
            {
                frameCount++;
            }
        }

        private void OnPostRenderHud(object sender, EventArgs e)
        {
            if (!Context.IsWorldReady) return;

            SpriteBatch spriteBatch = Game1.spriteBatch;
            spriteBatch.DrawString(
                Game1.smallFont,
                $"FPS: {currentFPS}",
                new Vector2(10, 10),
                Color.White
            );
        }
    }
}
