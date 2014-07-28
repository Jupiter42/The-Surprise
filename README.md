The-Surprise
============

//A C# XNA game. Meant to be a 2D first-person discovery game for Father's Day.
//Note: This was merely posted as a test.
//Have fun. Created for Windows system.

using System;
using System.Collections.Generic;
using System.Linq;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Audio;
using Microsoft.Xna.Framework.Content;
using Microsoft.Xna.Framework.GamerServices;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework.Input;
using Microsoft.Xna.Framework.Media;

namespace SecretSurprise
{
    /// <summary>
    /// This is the main type for your game
    /// </summary>
    /// 

    public class Game1 : Microsoft.Xna.Framework.Game
    {
        GraphicsDeviceManager graphics;
        SpriteBatch spriteBatch;
        //public static MouseState GetState();

        //Universal
        bool PlayerisSmoking = false;
        bool PlayerisPunching = false;
        bool IsgoingDown;
        bool CangoForward = false;
        bool CanContinue = false;
        bool CanEnter = false;
        bool HasKnocked = false;
        bool WolfAttackAOver = false;
        bool HasGenerated = false;
        bool HasAttacked = false;
        bool ShowNotice = true;
        int SmokingTimer = 0;
        int Timer = 0;
        int randomWolfAttack = 0;
        int randomWolfPosition;
        int hitsToWin = 0;
        int currentX;
        int LastState;
        float BlackOutAlpha = 0f;
        float Wolfattack2alpha = 1f;
        Song InGameSong;
        SoundEffect InGameSong3;
        SoundEffectInstance InGameSong3Instance;
        VideoPlayer player1;
        VideoPlayer player2;
        VideoPlayer player3;
        VideoPlayer player4;
        VideoPlayer player5;
        VideoPlayer player6;
        VideoPlayer player7;
        static Random random = new Random();
        Color TransparentColor = new Color(0, 0, 0, 255);
        Texture2D pipeTexture1;
        Texture2D pipeTexture2;
        Rectangle pipeRect1;
        BackgroundImageStruct Punch;
        BackgroundImageStruct BlackOut;
        BackgroundImageStruct WolfAttack1;
        BackgroundImageStruct WolfAttack2;
        BackgroundImageStruct Paused;
        ObjectCollisionStruct Collision1;
        ObjectCollisionStruct Collision2;
        ObjectCollisionStruct Collision3;
        ObjectCollisionStruct Collision4;
        ObjectCollisionStruct Collision5;

            //Game states
        int global_gameState = 1;
        const int MAIN_MENU = 1;
        const int MINI_GAME4 = 2;
        const int MINI_GAME1 = 3;
        const int MINI_GAME2 = 14;
        const int MINI_GAME3 = 15;
        const int SCENE1 = 4;
        const int SCENE2 = 5;
        const int SCENE3 = 6;
        const int SCENE4 = 7;
        const int SCENE5 = 8;
        const int SCENE6 = 9;
        const int SCENE7 = 10;
        const int SCENE8 = 11;
        const int SCENE9 = 12;
        const int CREDITS_SCENE = 13;
        const int PAUSED = 16;

        //Fonts
        SpriteFont Font1;
        Vector2 Font1Pos;
        string output = " ";

        //Main menu
        bool showdocumentation = false;
        BackgroundImageStruct Menu;
        BackgroundImageStruct Notice;
        Video Documentation;
        Texture2D DocumentationTex;
        Song Menusong;

        //Mini game1
        Song InGamesong2;
        bool StirLeft = false;
        int stirNum = 0;
        BackgroundImageStruct miniGame1;
        BackgroundImageStruct miniGame2;
        
        //Mini game2
        BackgroundImageStruct miniGame3;
        BackgroundImageStruct miniGame4;

        //Mini game3
        bool VideoisOver = false;
        bool isExtinguishing = false;
        BackgroundImageStruct miniGame5;
        BackgroundImageStruct miniFire1;
        BackgroundImageStruct miniFire2;
        BackgroundImageStruct miniFire3;
        BackgroundImageStruct miniFire4;
        BackgroundImageStruct miniCloud;
        BackgroundImageStruct miniExtinguisher;
        BackgroundImageStruct miniGameAfter;
        Video cutscene5;
        Texture2D videoTexture5 = null;
        Video cutscene6;
        Texture2D videoTexture6 = null;

        //Scene1
        //Texture2D sceneTexture;
        //Rectangle sceneRect;
        SoundEffect doorSound;
        SoundEffectInstance doorSoundInstance;
        BackgroundImageStruct Scene1;
        Video cutscene1;
        Texture2D videoTexture1 = null;

        //Scene2

        SoundEffect LegSnapSound;
        int zoomMultipleX = 0;
        int zoomMultipleY = 0;
        BackgroundImageStruct Scene2;
        BackgroundImageStruct Scene2Fall2;

        //Scene3
        BackgroundImageStruct Scene3;

        //Scene4
        BackgroundImageStruct Scene4;
        BackgroundImageStruct Scene4Man;
        BackgroundImageStruct OpenBible1A;
        BackgroundImageStruct OpenBible1B;
        int BibleState1 = 0;

        //Scene5
        BackgroundImageStruct Scene5;

        //Scene6
        BackgroundImageStruct Scene6;
        BackgroundImageStruct OpenBible2A;
        BackgroundImageStruct OpenBible2B;
        int BibleState2 = 0;

        //Scene7
        BackgroundImageStruct Scene7;
        BackgroundImageStruct Scene7Character;

        //Scene8
        BackgroundImageStruct Scene8;
        BackgroundImageStruct OpenBible3A;
        BackgroundImageStruct OpenBible3B;
        int BibleState3 = 0;

        //Scene9
        BackgroundImageStruct Scene9A;
        BackgroundImageStruct Scene9B;
        BackgroundImageStruct Key1;
        BackgroundImageStruct Key2;
        BackgroundImageStruct Key3;
        Video cutscene2;
        Video cutscene3;
        Texture2D videoTexture2 = null;
        Texture2D videoTexture3 = null;

        //Credits
        Video credits;
        Texture2D creditsTexture = null;

        //Cursor
        Texture2D cursorTexture;
        Rectangle cursorRect;
        int cursorX;
        int cursorY;
        MouseState ms = Mouse.GetState();
        int curMousePosX;
        int curMousePosY;

        //Center building point of the world
        int CenteroftheUniverseX = -430;
        int CenteroftheUniverseY = -460;

        public Game1()
        {
            graphics = new GraphicsDeviceManager(this);
            Content.RootDirectory = "Content";
            this.graphics.PreferredBackBufferWidth = 640;
            this.graphics.PreferredBackBufferHeight = 480;

            this.graphics.IsFullScreen = true;
        }

        struct BackgroundImageStruct
        {
            public Texture2D BackgroundTexture;
            public Rectangle BackgroundRect;
            public int FireX;
            public int FireY;
        }

        struct ObjectCollisionStruct
        {
            public Texture2D CollisionTexture;
            public Rectangle CollisionRect;
            //public ObjectCollisionStruct door1Collision;
        }


        /// <summary>
        /// Allows the game to perform any initialization it needs to before starting to run.
        /// This is where it can query for any required services and load any non-graphic
        /// related content.  Calling base.Initialize will enumerate through any components
        /// and initialize them as well.
        /// </summary>
        protected override void Initialize()
        {
            // TODO: Add your initialization logic here+

            //gameSpriteBatch = new SpriteBatch(graphics.GraphicsDevice);

            //Cursor
            cursorX = GraphicsDevice.Viewport.Width / 2;
            cursorY = GraphicsDevice.Viewport.Height / 2;
            cursorRect = new Rectangle(cursorX, cursorY, 20, 20);

            //Menu
            Notice.BackgroundRect = new Rectangle(0, 0,
                            GraphicsDevice.Viewport.Width, GraphicsDevice.Viewport.Height);

            //MiniGame
            miniGame1.BackgroundRect = new Rectangle(0, 0,
                            GraphicsDevice.Viewport.Width, GraphicsDevice.Viewport.Height);

            miniGame2.BackgroundRect = new Rectangle(0, 0,
                            GraphicsDevice.Viewport.Width, GraphicsDevice.Viewport.Height);

            miniGame3.BackgroundRect = new Rectangle(0, 0,
                            GraphicsDevice.Viewport.Width, GraphicsDevice.Viewport.Height);

            miniGame4.BackgroundRect = new Rectangle(0, 0,
                            GraphicsDevice.Viewport.Width, GraphicsDevice.Viewport.Height);

            miniGameAfter.BackgroundRect = new Rectangle(0, 0,
                            GraphicsDevice.Viewport.Width, GraphicsDevice.Viewport.Height);

            miniFire1.BackgroundRect = new Rectangle(CenteroftheUniverseX + miniFire1.FireX,
    CenteroftheUniverseY + miniFire1.FireY,
        GraphicsDevice.Viewport.Width * 139 / 1280, GraphicsDevice.Viewport.Height * 232 / 960);

            miniFire2.BackgroundRect = new Rectangle(CenteroftheUniverseX + miniFire2.FireX,
                CenteroftheUniverseY + miniFire2.FireY,
                    GraphicsDevice.Viewport.Width * 139 / 1280, GraphicsDevice.Viewport.Height * 232 / 960);

            miniFire3.BackgroundRect = new Rectangle(CenteroftheUniverseX + miniFire3.FireX,
                CenteroftheUniverseY + miniFire3.FireY,
                    GraphicsDevice.Viewport.Width * 139 / 1280, GraphicsDevice.Viewport.Height * 232 / 960);

            miniFire4.BackgroundRect = new Rectangle(CenteroftheUniverseX + miniFire4.FireX,
                CenteroftheUniverseY + miniFire4.FireY,
                    GraphicsDevice.Viewport.Width * 139 / 1280, GraphicsDevice.Viewport.Height * 232 / 960);

            miniExtinguisher.BackgroundRect = new Rectangle(GraphicsDevice.Viewport.Width * 190 / 640, GraphicsDevice.Viewport.Height * 327 / 480,
                            GraphicsDevice.Viewport.Width * 303 / 640,
                GraphicsDevice.Viewport.Height * 154 / 480);
             
            //Scene2
            Scene2Fall2.BackgroundRect = new Rectangle(1, 1,
                            GraphicsDevice.Viewport.Width, GraphicsDevice.Viewport.Height);

            //Scene9
            Key1.BackgroundRect = new Rectangle(GraphicsDevice.Viewport.Width * 5 * 152 / 3200, 
                GraphicsDevice.Viewport.Height * 3 * 227 / 1400, 15, 16);

            Key2.BackgroundRect = new Rectangle(GraphicsDevice.Viewport.Width * 5 * 282 / 3200,
                GraphicsDevice.Viewport.Height * 3 * 280 / 1400, 15, 16);

            Key3.BackgroundRect = new Rectangle(GraphicsDevice.Viewport.Width * 5 * 386 / 3200,
                GraphicsDevice.Viewport.Height * 3 * 226 / 1400, 15, 16);

            //Universal
            int pipeX = GraphicsDevice.Viewport.Width - 234;
            int pipeY = GraphicsDevice.Viewport.Height - 155;
            pipeRect1 = new Rectangle(pipeX, pipeY, 234, 155);
            Punch.BackgroundRect = new Rectangle(GraphicsDevice.Viewport.Width - 174, GraphicsDevice.Viewport.Height, 174, 236);
            BlackOut.BackgroundRect = new Rectangle(1, 1, GraphicsDevice.Viewport.Width, GraphicsDevice.Viewport.Height);
            OpenBible1A.BackgroundRect = new Rectangle(1, 1,
                            GraphicsDevice.Viewport.Width, GraphicsDevice.Viewport.Height);

            Paused.BackgroundRect = new Rectangle(0, 0, GraphicsDevice.Viewport.Width, GraphicsDevice.Viewport.Height);

            base.Initialize();
        }



        /// <summary>
        /// LoadContent will be called once per game and is the place to load
        /// all of your content.
        /// </summary>
        protected override void LoadContent()
        {
            // Create a new SpriteBatch, which can be used to draw textures.
            spriteBatch = new SpriteBatch(GraphicsDevice);

            // TODO: use this.Content to load your game content here.

            //Cursor
            cursorTexture = this.Content.Load<Texture2D>("SSCursor");

            spriteBatch = new SpriteBatch(GraphicsDevice);
            
            player1 = new VideoPlayer();
            player2 = new VideoPlayer();
            player3 = new VideoPlayer();
            player4 = new VideoPlayer();
            player5 = new VideoPlayer();
            player6 = new VideoPlayer();
            player7 = new VideoPlayer();

            //Load Screen
            spriteBatch.Begin();
            spriteBatch.Draw(this.Content.Load<Texture2D>("SSLoadingScreen"),
                new Rectangle(1, 1, GraphicsDevice.Viewport.Width, GraphicsDevice.Viewport.Height), Color.White);
            spriteBatch.End();

            //Fonts
            Font1 = Content.Load<SpriteFont>("Pesca");
            
            Font1Pos = new Vector2(1, GraphicsDevice.Viewport.Height - 20);

            //Menu
            Menu.BackgroundTexture = this.Content.Load<Texture2D>("SSMenu");
            Notice.BackgroundTexture = this.Content.Load<Texture2D>("SSNotice");
            Documentation = Content.Load<Video>("SecretSurpriseDocumentation");

            //Mini Game
            miniGame1.BackgroundTexture = this.Content.Load<Texture2D>("SSF1");
            miniGame2.BackgroundTexture = this.Content.Load<Texture2D>("SSF2");
            miniGame3.BackgroundTexture = this.Content.Load<Texture2D>("SSF3");
            miniGame4.BackgroundTexture = this.Content.Load<Texture2D>("SSF4");
            miniGame5.BackgroundTexture = this.Content.Load<Texture2D>("SSF5");
            miniCloud.BackgroundTexture = this.Content.Load<Texture2D>("SSF7");

            miniGameAfter.BackgroundTexture = this.Content.Load<Texture2D>("SSF9");

            miniExtinguisher.BackgroundTexture = this.Content.Load<Texture2D>("SSF6");
            miniFire1.BackgroundTexture = this.Content.Load<Texture2D>("SSF8");
            cutscene5 = Content.Load<Video>("SSMiniGame");
            cutscene6 = Content.Load<Video>("SSMiniGame2");

            //Scene 1
            Scene1.BackgroundTexture = this.Content.Load<Texture2D>("SS1");
            cutscene1 = Content.Load<Video>("SSCutScene1");

            //Scene2
            Scene2.BackgroundTexture = this.Content.Load<Texture2D>("SS2");
            Scene2Fall2.BackgroundTexture = this.Content.Load<Texture2D>("SSFalling2");

            //Scene3
            Scene3.BackgroundTexture = this.Content.Load<Texture2D>("SS3");

            //Scene4
            Scene4.BackgroundTexture = this.Content.Load < Texture2D>("SS4");
            Scene4Man.BackgroundTexture = this.Content.Load<Texture2D>("SS4Man");
            OpenBible1A.BackgroundTexture = this.Content.Load<Texture2D>("SSKey1A");
            OpenBible1B.BackgroundTexture = this.Content.Load<Texture2D>("SSKey1B");

            //Scene5
            Scene5.BackgroundTexture = this.Content.Load<Texture2D>("SS5");

            //Scene6
            Scene6.BackgroundTexture = this.Content.Load<Texture2D>("SS6");
            OpenBible2A.BackgroundTexture = this.Content.Load<Texture2D>("SSKey2A");
            OpenBible2B.BackgroundTexture = this.Content.Load<Texture2D>("SSKey2B");

            //Scene7
            Scene7.BackgroundTexture = this.Content.Load<Texture2D>("SS7");
            Scene7Character.BackgroundTexture = this.Content.Load<Texture2D>("SSLibrarian");

            //Scene8
            Scene8.BackgroundTexture = this.Content.Load<Texture2D>("SS8");
            OpenBible3A.BackgroundTexture = this.Content.Load<Texture2D>("SSKey3A");
            OpenBible3B.BackgroundTexture = this.Content.Load<Texture2D>("SSKey3B");

            //Scene9
            Scene9A.BackgroundTexture = this.Content.Load<Texture2D>("SS9A");
            Scene9B.BackgroundTexture = this.Content.Load<Texture2D>("SS9B");
            Key1.BackgroundTexture = this.Content.Load<Texture2D>("SSKey1");
            Key2.BackgroundTexture = this.Content.Load<Texture2D>("SSKey2");
            Key3.BackgroundTexture = this.Content.Load<Texture2D>("SSKey3");
            cutscene2 = Content.Load<Video>("SSCutScene2");
            cutscene3 = Content.Load<Video>("SSCutScene3");

            //Credits
            credits = Content.Load<Video>("SSCredits");

            //Universal
            Punch.BackgroundTexture = this.Content.Load<Texture2D>("SSFist");
            pipeTexture1 = this.Content.Load<Texture2D>("SSPipe1");
            pipeTexture2 = this.Content.Load<Texture2D>("SSPipe2");
            BlackOut.BackgroundTexture = this.Content.Load<Texture2D>("SSBlackout");

            Collision1.CollisionTexture = this.Content.Load<Texture2D>("SSHitBox");
            Collision2.CollisionTexture = this.Content.Load<Texture2D>("SSHitBox");
            Collision3.CollisionTexture = this.Content.Load<Texture2D>("SSHitBox");
            Collision4.CollisionTexture = this.Content.Load<Texture2D>("SSHitBox");
            Collision5.CollisionTexture = this.Content.Load<Texture2D>("SSHitBox");

            WolfAttack1.BackgroundTexture = this.Content.Load<Texture2D>("SSWolf");
            WolfAttack2.BackgroundTexture = this.Content.Load<Texture2D>("SSWolf3");

            Paused.BackgroundTexture = this.Content.Load<Texture2D>("SSPaused");

            Menusong = Content.Load<Song>("SSMenusong");
            InGameSong = Content.Load<Song>("SSInGameSong1");
            InGamesong2=Content.Load<Song>("SSInGameSong2");
            InGameSong3 = Content.Load<SoundEffect>("SSInGameSong3");
            InGameSong3Instance = InGameSong3.CreateInstance();
            LegSnapSound = Content.Load<SoundEffect>("Whack");
            doorSound = Content.Load<SoundEffect>("LockedDoor");
            doorSoundInstance = doorSound.CreateInstance();

            MediaPlayer.Play(Menusong);    
        }

        /// <summary>
        /// UnloadContent will be called once per game and is the place to unload
        /// all content.
        /// </summary>
        protected override void UnloadContent()
        {
            // TODO: Unload any non ContentManager content here
        }

        /// <summary>
        /// Allows the game to run logic such as updating the world,
        /// checking for collisions, gathering input, and playing audio.
        /// </summary>
        /// <param name="gameTime">Provides a snapshot of timing values.</param>
        protected override void Update(GameTime gameTime)
        {
            KeyboardState keys = Keyboard.GetState();
            GamePadState gamePad1 = GamePad.GetState(PlayerIndex.One);

            if(global_gameState != PAUSED)
            if (keys.IsKeyDown(Keys.Escape) || GamePad.GetState(PlayerIndex.One).Buttons.Start == ButtonState.Pressed
                || keys.IsKeyDown(Keys.P) || keys.IsKeyDown(Keys.M))
            {
                MediaPlayer.Pause();
                LastState = global_gameState;
                global_gameState = PAUSED;
            }

            GamePad.SetVibration(PlayerIndex.One, 0, 0);

            switch (global_gameState)
            {
                case MAIN_MENU:

                    if(MediaPlayer.State != MediaState.Playing)
                    MediaPlayer.Play(Menusong);

                    Menu.BackgroundRect = new Rectangle(CenteroftheUniverseX, CenteroftheUniverseY,
                        GraphicsDevice.Viewport.Width * 5, GraphicsDevice.Viewport.Height * 3);

                    Collision1.CollisionRect = new Rectangle(CenteroftheUniverseX + GraphicsDevice.Viewport.Width * 5 * 693 / 3200,
                        CenteroftheUniverseY + GraphicsDevice.Viewport.Height * 3 * 860 / 1440,
                        GraphicsDevice.Viewport.Width * 5 * 100 / 3200, GraphicsDevice.Viewport.Height * 3 * 25 / 1440);

                    Collision2.CollisionRect = new Rectangle(CenteroftheUniverseX + GraphicsDevice.Viewport.Width * 5 * 1393 / 3200,
                        CenteroftheUniverseY + GraphicsDevice.Viewport.Height * 3 * 373 / 1440,
                        GraphicsDevice.Viewport.Width * 5 * 153 / 3200, GraphicsDevice.Viewport.Height * 3 * 31 / 1440);

                    Collision3.CollisionRect = new Rectangle(CenteroftheUniverseX + GraphicsDevice.Viewport.Width * 5 * 1230 / 3200,
                        CenteroftheUniverseY + GraphicsDevice.Viewport.Height * 3 * 1032 / 1440,
                        GraphicsDevice.Viewport.Width * 5 * 77 / 3200, GraphicsDevice.Viewport.Height * 3 * 27 / 1440);

                    Collision4.CollisionRect = new Rectangle(CenteroftheUniverseX + GraphicsDevice.Viewport.Width * 5 * 1713 / 3200,
                        CenteroftheUniverseY + GraphicsDevice.Viewport.Height * 3 * 757 / 1440,
                        GraphicsDevice.Viewport.Width * 5 * 119 / 3200, GraphicsDevice.Viewport.Height * 3 * 33 / 1440);

                    realGameMovementupdate();

                    if (Collision1.CollisionRect.Intersects(cursorRect))
                    {
                        output = "Left Click/A: Select";

                        if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                            || keys.IsKeyDown(Keys.A))
                        {
                            showdocumentation = true;
                            output = "";
                            player7.Play(Documentation);
                        }
                    }

                    if (Collision2.CollisionRect.Intersects(cursorRect))
                    {
                        output = "Left Click/A: Select";

                        if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                            || keys.IsKeyDown(Keys.A))
                        {
                            output = "";
                            CenteroftheUniverseX = 0;
                            CenteroftheUniverseY = 0;
                            StirLeft = false;
                            stirNum = 0;
                            MediaPlayer.Stop();
                            MediaPlayer.Play(InGamesong2);
                            global_gameState = MINI_GAME1;
                        }
                    }

                    if (Collision3.CollisionRect.Intersects(cursorRect))
                    {
                        output = "Left Click/A: Select";

                        if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                            || keys.IsKeyDown(Keys.A))
                        {
                            output = "";
                            global_gameState = CREDITS_SCENE;
                            MediaPlayer.Stop();
                            player4.Play(credits);
                        }
                    }

                    if (Collision4.CollisionRect.Intersects(cursorRect))
                    {
                        output = "Left Click/A: Select";

                        if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                            || keys.IsKeyDown(Keys.A))
                        {
                            output = "";
                            CenteroftheUniverseX = -1239;
                            CenteroftheUniverseY = -187;

                            global_gameState = SCENE1;
                            MediaPlayer.Stop();
                            MediaPlayer.Play(InGameSong);
                            player1.Play(cutscene1);
                        }
                    }

                    if (ShowNotice == true)
                    {
                        CenteroftheUniverseX = -430;
                        CenteroftheUniverseY = -460;

                        if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                            || keys.IsKeyDown(Keys.A))
                        ShowNotice = false;
                    }

                    if (showdocumentation == true)
                    {
                        DocumentationTex = null;
                        if (player7.State != MediaState.Stopped)
                        {
                            CenteroftheUniverseX = -430;
                            CenteroftheUniverseY = -460;
                            output = "";
                            DocumentationTex = player7.GetTexture();
                        }

                        else
                        {
                            output = "Right Click/A: Stir Pancake Mix";
                            CenteroftheUniverseX = 0;
                            CenteroftheUniverseY = 0;
                            StirLeft = true;
                            stirNum = 0;
                            MediaPlayer.Stop();
                            MediaPlayer.Play(InGamesong2);
                            showdocumentation = false;
                            global_gameState = MINI_GAME1;
                        }
                    }

                    break;

                case MINI_GAME1:

                    curMousePosX = ms.X;
            ms = Mouse.GetState();
            curMousePosY = ms.Y;
            Mouse.SetPosition(cursorX, cursorY);

            if (MediaPlayer.State != MediaState.Playing)
                MediaPlayer.Play(InGamesong2);

                        if(StirLeft == false)
                        if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                                || keys.IsKeyDown(Keys.A))
                        {
                            output = "Right Click/B: Stir Pancake Mix";
                            StirLeft = true;
                            stirNum++;
                        }

                            if(StirLeft == true)
                         if (ms.RightButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.B == ButtonState.Pressed
                                || keys.IsKeyDown(Keys.B))
                        {
                            //output = "Moo.";
                            output = "Left Click/A: Stir Pancake Mix";
                            StirLeft = false;
                            stirNum++;
                        }


                            if (stirNum >= 15)
                            {
                                //CenteroftheUniverseX = -430;
                                //CenteroftheUniverseY = -460;
                                StirLeft = false;
                                stirNum = 0;
                                global_gameState = MINI_GAME2;
                            }

                    break;

                case MINI_GAME2:

                    curMousePosX = ms.X;
            ms = Mouse.GetState();
            curMousePosY = ms.Y;
            Mouse.SetPosition(cursorX, cursorY);

                    if(MediaPlayer.State != MediaState.Playing)
                    MediaPlayer.Play(InGamesong2);

                    if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                                || keys.IsKeyDown(Keys.A))
                    {
                        stirNum++;
                        StirLeft = false;
                    }

                    else
                    {
                        StirLeft = true;
                    }

                    output = "Left Click/A: Pour Pancake Mix";

                    if (stirNum >= 100)
                    {
                        isExtinguishing = false;
                        CenteroftheUniverseY = 0;
                        stirNum = 0;
                        miniFire1.FireX = fireXgenerator(miniFire1.FireX);
                        miniFire2.FireX = fireXgenerator(miniFire2.FireX);
                        miniFire3.FireX = fireXgenerator(miniFire3.FireX);
                        miniFire4.FireX = fireXgenerator(miniFire4.FireX);

                        miniFire1.FireY = fireYgenerator(miniFire1.BackgroundRect.Height, miniFire1.FireY);
                        miniFire2.FireY = fireYgenerator(miniFire2.BackgroundRect.Height, miniFire2.FireY);
                        miniFire3.FireY = fireYgenerator(miniFire3.BackgroundRect.Height, miniFire3.FireY);
                        miniFire4.FireY = fireYgenerator(miniFire4.BackgroundRect.Height, miniFire4.FireY);

                        miniFire1.BackgroundRect.X = CenteroftheUniverseX + miniFire1.FireX;
                        miniFire2.BackgroundRect.X = CenteroftheUniverseX + miniFire2.FireX;
                        miniFire3.BackgroundRect.X = CenteroftheUniverseX + miniFire3.FireX;
                        miniFire4.BackgroundRect.X = CenteroftheUniverseX + miniFire4.FireX;

                        miniFire1.BackgroundRect.Width = GraphicsDevice.Viewport.Width * 139 / 1280;
                        miniFire1.BackgroundRect.Height = GraphicsDevice.Viewport.Height * 232 / 960;
                        miniFire2.BackgroundRect.Width = GraphicsDevice.Viewport.Width * 139 / 1280;
                        miniFire2.BackgroundRect.Height = GraphicsDevice.Viewport.Height * 232 / 960;
                        miniFire3.BackgroundRect.Width = GraphicsDevice.Viewport.Width * 139 / 1280;
                        miniFire3.BackgroundRect.Height = GraphicsDevice.Viewport.Height * 232 / 960;
                        miniFire4.BackgroundRect.Width = GraphicsDevice.Viewport.Width * 139 / 1280;
                        miniFire4.BackgroundRect.Height = GraphicsDevice.Viewport.Height * 232 / 960;

                        VideoisOver = false;

                        global_gameState = MINI_GAME3;
                        player5.Play(cutscene5);
                    }

                    break;

                case MINI_GAME3:

                    if(MediaPlayer.State != MediaState.Playing)
                    MediaPlayer.Play(InGamesong2);

                    if (VideoisOver == false)
                    {
                        videoTexture5 = null;
                        if (player5.State != MediaState.Stopped)
                        {
                            output = "";
                            videoTexture5 = player5.GetTexture();
                        }

                        else
                        {
                            output = "Left click to extinguish fires. Don't let the flames get too large. Shift/X: Continue";

                            if (GamePad.GetState(PlayerIndex.One).Buttons.X == ButtonState.Pressed
                        || keys.IsKeyDown(Keys.LeftShift) || keys.IsKeyDown(Keys.RightShift) || keys.IsKeyDown(Keys.X))
                                VideoisOver = true;

                        }
                    }

                    if (VideoisOver == true)
                    {
                        isExtinguishing = false;

                        miniGame5.BackgroundRect = new Rectangle(CenteroftheUniverseX, CenteroftheUniverseY,
                                GraphicsDevice.Viewport.Width * 4, GraphicsDevice.Viewport.Height);

                        miniFire1.BackgroundRect.X = CenteroftheUniverseX + miniFire1.FireX;
                        miniFire2.BackgroundRect.X = CenteroftheUniverseX + miniFire2.FireX;
                        miniFire3.BackgroundRect.X = CenteroftheUniverseX + miniFire3.FireX;
                        miniFire4.BackgroundRect.X = CenteroftheUniverseX + miniFire4.FireX;

                        miniFire1.BackgroundRect.Y = CenteroftheUniverseY + miniFire1.FireY;
                        miniFire2.BackgroundRect.Y = CenteroftheUniverseY + miniFire2.FireY;
                        miniFire3.BackgroundRect.Y = CenteroftheUniverseY + miniFire3.FireY;
                        miniFire4.BackgroundRect.Y = CenteroftheUniverseY + miniFire4.FireY;

                        miniCloud.BackgroundRect = new Rectangle(miniExtinguisher.BackgroundRect.X - (GraphicsDevice.Viewport.Width * 83 / 640),
                            GraphicsDevice.Viewport.Height * 289 / 480,
                                GraphicsDevice.Viewport.Width * 233 / 640,
                    GraphicsDevice.Viewport.Height * 191 / 480);

                        fakeGameMovementupdate();
                        output = stirNum.ToString();

                        if (global_gameState == MINI_GAME4)
                        {
                            GamePad.SetVibration(PlayerIndex.One, 1, 1);
                            VideoisOver = false;
                            GamePad.SetVibration(PlayerIndex.One, 1, 1);
                            player6.Play(cutscene6);
                        }
                    }
                    break;

                case MINI_GAME4:

                    if (MediaPlayer.State != MediaState.Playing)
                        MediaPlayer.Play(InGamesong2);
                    curMousePosX = ms.X;
            ms = Mouse.GetState();
            curMousePosY = ms.Y;
            Mouse.SetPosition(cursorX, cursorY);

                    if (VideoisOver == false)
                    {
                        videoTexture6 = null;
                        if (player6.State != MediaState.Stopped)
                        {
                            output = "";
                            videoTexture6 = player6.GetTexture();
                        }

                        else
                        {
                            VideoisOver = true;
                        }
                    }

                    else if (VideoisOver == true)
                    {
                        output = "Score: " + stirNum.ToString();

                        if (ms.RightButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.B == ButtonState.Pressed
                            || keys.IsKeyDown(Keys.B))
                        {
                            CenteroftheUniverseX = -430;
                            CenteroftheUniverseY = -460;
                            MediaPlayer.Stop();
                            MediaPlayer.Play(Menusong);
                            global_gameState = MAIN_MENU;
                        }

                        else if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                            || keys.IsKeyDown(Keys.A))
                        {
                            output = "";
                            CenteroftheUniverseX = -1239;
                            CenteroftheUniverseY = -187;

                            global_gameState = SCENE1;
                            MediaPlayer.Stop();
                            MediaPlayer.Play(InGameSong);
                            player1.Play(cutscene1);
                        }
                    }

                    break;

                case SCENE1:

                    if (MediaPlayer.State != MediaState.Playing)
                        MediaPlayer.Play(InGameSong);

                    Scene1.BackgroundRect = new Rectangle(CenteroftheUniverseX, CenteroftheUniverseY, 
                        GraphicsDevice.Viewport.Width * 5, GraphicsDevice.Viewport.Height * 3);
                    
                 Collision1.CollisionRect = new Rectangle(CenteroftheUniverseX + GraphicsDevice.Viewport.Width * 5 * 243 / 320, 
                        CenteroftheUniverseY + GraphicsDevice.Viewport.Height * 3* 54 / 144, 
                        GraphicsDevice.Viewport.Width * 5 * 265 / 3200, GraphicsDevice.Viewport.Height * 3 * 369 / 1440);
                    
                  Collision2.CollisionRect = new Rectangle(CenteroftheUniverseX + GraphicsDevice.Viewport.Width * 5 * 413 / 3200,
                        CenteroftheUniverseY + GraphicsDevice.Viewport.Height * 3 * 533 / 1440,
                        GraphicsDevice.Viewport.Width * 5 * 1693 / 3200, GraphicsDevice.Viewport.Height * 3 * 341 / 1440);

                    realGameMovementupdate();

                    if(Collision1.CollisionRect.Intersects(cursorRect))
                    {
                        forwardText();

                        if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed 
                            || keys.IsKeyDown(Keys.A))
                        {
                            if(doorSoundInstance.State != SoundState.Playing)
                            doorSoundInstance.Play();

                            output = "This door is locked. Looks like there's only one way to go.....";
                            CangoForward = true;
                        }
                    }

                    if (Collision2.CollisionRect.Intersects(cursorRect))
                    {
                        forwardText();

                        if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                            || keys.IsKeyDown(Keys.A) && CangoForward == false)
                        {
                            output = "I'm not jumping off the balcony...";
                        }

                        if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                            || keys.IsKeyDown(Keys.A) && CangoForward == true)
                        {
                            if (CangoForward == true)
                            {
                                CanContinue = true;
                            }
                        }
                    }

                    if (CanContinue == true)
                    {
                        output = "Well....... Here we go.....";

                        CangoForward = false;
                        BlackOutAlpha = 1f;

                        fallingAnimation();
                        //CangoForward = false;
                        //CanContinue = false;
                    }

                    videoTexture1 = null;
                    if (player1.State != MediaState.Stopped)
                    {
                        output = "";
                        videoTexture1 = player1.GetTexture();
                        CenteroftheUniverseX = -1239;
                        CenteroftheUniverseY = -187;
                    }

                    break;


                case SCENE2:

                    if (MediaPlayer.State != MediaState.Playing)
                        MediaPlayer.Play(InGameSong);

            output = " ";

            curMousePosX = ms.X;
            ms = Mouse.GetState();
            curMousePosY = ms.Y;
                    Mouse.SetPosition(cursorX, cursorY);
                    if (keys.IsKeyDown(Keys.Space) || GamePad.GetState(PlayerIndex.One).Buttons.Y == ButtonState.Pressed)
            {
                PlayerisSmoking = true;
            }

                else
            {
                PlayerisSmoking = false;
            }

            if (ms.RightButton == ButtonState.Pressed || 
                keys.IsKeyDown(Keys.B) || GamePad.GetState(PlayerIndex.One).Buttons.B == ButtonState.Pressed && PlayerisPunching == false)
            {
                PlayerisPunching = true;
            }
                    if (CanContinue == false)
                    {
                        Scene2.BackgroundRect = new Rectangle(CenteroftheUniverseX, CenteroftheUniverseY,
                            GraphicsDevice.Viewport.Width * 5, GraphicsDevice.Viewport.Height * 5);

                        if (Scene2.BackgroundRect.Bottom > GraphicsDevice.Viewport.Height - GraphicsDevice.Viewport.Width / 160)
                        {
                            CenteroftheUniverseY -= GraphicsDevice.Viewport.Width / 160 * 2;
                        }

                        else
                        {
                            for (int Count = 0; Count < 500; Count++)
                            {

                            }

                            CanContinue = true;
                        }
                    }

                    else if (CanContinue == true && CangoForward == false)
                    {
                        if (Scene2Fall2.BackgroundRect.Width < GraphicsDevice.Viewport.Width * GraphicsDevice.Viewport.Width)
                        {
                            Scene2Fall2.BackgroundRect.Width += ((GraphicsDevice.Viewport.Width / 160) * 4) + zoomMultipleX;
                            Scene2Fall2.BackgroundRect.Height += ((GraphicsDevice.Viewport.Height / 160) * 4) + zoomMultipleY;
                            Scene2Fall2.BackgroundRect.X = -Scene2Fall2.BackgroundRect.Width / 2 + (GraphicsDevice.Viewport.Width / 2);
                            Scene2Fall2.BackgroundRect.Y = -Scene2Fall2.BackgroundRect.Height / 2 + (GraphicsDevice.Viewport.Height / 2);
                            zoomMultipleX += GraphicsDevice.Viewport.Width / 160;
                            zoomMultipleY += GraphicsDevice.Viewport.Height / 160;
                        }

                        else
                        {
                            CangoForward = true;
                            GamePad.SetVibration(PlayerIndex.One, 0, 0);
                            LegSnapSound.Play();
                        }
                    }

                    else if (CangoForward == true)
                    {
                        CenteroftheUniverseX = -779;
                        CenteroftheUniverseY = -477;
                        Scene3.BackgroundRect = new Rectangle(CenteroftheUniverseX, CenteroftheUniverseY,
                            GraphicsDevice.Viewport.Width * 5, GraphicsDevice.Viewport.Height * 3);

                            spriteBatch.Begin();
                            spriteBatch.Draw(Scene3.BackgroundTexture, Scene3.BackgroundRect, Color.White);
                            spriteBatch.Draw(BlackOut.BackgroundTexture, BlackOut.BackgroundRect, Color.Red*BlackOutAlpha);
                            spriteBatch.End();

                        output = " ";
                            if (BlackOutAlpha > 0f)
                            {
                                BlackOutAlpha -= 0.005f;
                            }

                            else if (BlackOutAlpha <= 0f)
                            {
                                CanContinue = false;
                                CangoForward = false;
                                global_gameState = SCENE3;
                                BlackOutAlpha = 0f;
                            }
                    }

                    break;

                case SCENE3:

                    if (MediaPlayer.State != MediaState.Playing)
                        MediaPlayer.Play(InGameSong);

                        Timer++;
                        Scene3.BackgroundRect = new Rectangle(CenteroftheUniverseX, CenteroftheUniverseY,
                            GraphicsDevice.Viewport.Width * 5, GraphicsDevice.Viewport.Height * 3);

                        Collision1.CollisionRect = new Rectangle(CenteroftheUniverseX + GraphicsDevice.Viewport.Width * 5 * 765 / 3200,
                        CenteroftheUniverseY + GraphicsDevice.Viewport.Height * 3 * 601 / 1440,
                        GraphicsDevice.Viewport.Width * 5 * 641 / 3200, GraphicsDevice.Viewport.Height * 3 * 121 / 1440);

                        Collision2.CollisionRect = new Rectangle(CenteroftheUniverseX + GraphicsDevice.Viewport.Width * 5 * 1809 / 3200,
                        CenteroftheUniverseY + GraphicsDevice.Viewport.Height * 3 * 537 / 1440,
                        GraphicsDevice.Viewport.Width * 5 * 361 / 3200, GraphicsDevice.Viewport.Height * 3 * 545 / 1440);

                        Collision3.CollisionRect = new Rectangle(CenteroftheUniverseX + GraphicsDevice.Viewport.Width * 5 * 1985 / 3200,
                        CenteroftheUniverseY + GraphicsDevice.Viewport.Height * 3 * 1193 / 1440,
                        GraphicsDevice.Viewport.Width * 5 * 257 / 3200, GraphicsDevice.Viewport.Height * 3 * 113 / 1440);

                        realGameMovementupdate();

                        if (Collision1.CollisionRect.Intersects(cursorRect))
                        {
                            output = "There's a poorly drawn city over there. Left Click/A: Forward";
                            if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                            || keys.IsKeyDown(Keys.A))
                            {
                                     CangoForward = false;
                                     CanContinue = false;
                                     CanEnter = false;
                                     CenteroftheUniverseX = -491;
                                     CenteroftheUniverseY = -419;
                                     global_gameState = SCENE5;
                            }

                        }

                        if (Collision2.CollisionRect.Intersects(cursorRect))
                        {
                                output = "Alright, this is a creepy little hut..... Left Click/A: Forward";
                               if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                                || keys.IsKeyDown(Keys.A))
                                {
                                    CenteroftheUniverseX = -1091;
                                    CenteroftheUniverseY = -511;
                                    global_gameState = SCENE4;
                                }
                        }

                        if (Collision3.CollisionRect.Intersects(cursorRect))
                        {
                            output = "Who would write 'Death can see' in red paint?";
                            if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                            || keys.IsKeyDown(Keys.A))
                            {
                                output = "What do you expect me to do? Chisel out a chunk of the street? No.";
                            }
                        }

                    break;

                case SCENE4:

                    if (MediaPlayer.State != MediaState.Playing)
                        MediaPlayer.Play(InGameSong);

                    Scene4.BackgroundRect = new Rectangle(CenteroftheUniverseX, CenteroftheUniverseY,
                            GraphicsDevice.Viewport.Width * 5, GraphicsDevice.Viewport.Height * 3);

                    Scene4Man.BackgroundRect = new Rectangle(CenteroftheUniverseX + GraphicsDevice.Viewport.Width * 5 * 1420 / 3200,
                        CenteroftheUniverseY + GraphicsDevice.Viewport.Height * 3 * 550 / 1440,
                        GraphicsDevice.Viewport.Width * 5 * 285 / 3200, GraphicsDevice.Viewport.Height * 3 * 360 / 1440);

                    Collision2.CollisionRect = new Rectangle(CenteroftheUniverseX + GraphicsDevice.Viewport.Width * 5 * 2461 / 3200,
                        CenteroftheUniverseY + GraphicsDevice.Viewport.Height * 3 * 497 / 1440,
                        GraphicsDevice.Viewport.Width * 5 * 353 / 3200, GraphicsDevice.Viewport.Height * 3 * 457 / 1440);

                    Collision1.CollisionRect = new Rectangle(CenteroftheUniverseX + GraphicsDevice.Viewport.Width * 5 * 451 / 3200,
                        CenteroftheUniverseY + GraphicsDevice.Viewport.Height * 3 * 668 / 1440,
                        GraphicsDevice.Viewport.Width * 5 * 42 / 3200, GraphicsDevice.Viewport.Height * 3 * 50 / 1440);

                    realGameMovementupdate();

                    if (Collision2.CollisionRect.Intersects(cursorRect))
                    {
                        forwardText();
                        if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                        || keys.IsKeyDown(Keys.A))
                        {
                            CangoForward = false;
                            CanContinue = false;
                            CanEnter = false;
                            CenteroftheUniverseX = -121;
                            CenteroftheUniverseY = -625;
                            global_gameState = SCENE3;
                        }
                    }

                    if (Collision1.CollisionRect.Intersects(cursorRect))
                    {
                        itemText();
                        if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                        || keys.IsKeyDown(Keys.A))
                        {
                            if(HasKnocked == false)
                            output = "I don't need this!";

                            else if (HasKnocked == true)
                            {
                                if (BibleState1 == 0)
                                    BibleState1 = 1;
                            }
                        }
                    }

                    else if (BibleState1 == 1)
                            BibleState1 = 2;

                    if (Scene4Man.BackgroundRect.Intersects(cursorRect))
                    {
                        talkText();
                        if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                        || keys.IsKeyDown(Keys.A))
                        {
                            output = "Stranger: Your 'Father', eh? Is he that Pastor at the local church?";
                        }
                    }

                    break;

                case SCENE5:

                    if (MediaPlayer.State != MediaState.Playing)
                        MediaPlayer.Play(InGameSong);

                    Scene5.BackgroundRect = new Rectangle(CenteroftheUniverseX, CenteroftheUniverseY,
                            GraphicsDevice.Viewport.Width * 5, GraphicsDevice.Viewport.Height * 3);

                    Collision1.CollisionRect = new Rectangle(CenteroftheUniverseX + GraphicsDevice.Viewport.Width * 5 * 1915 / 3200,
                        CenteroftheUniverseY + GraphicsDevice.Viewport.Height * 3 * 471 / 1440,
                        GraphicsDevice.Viewport.Width * 5 * 47 / 3200, GraphicsDevice.Viewport.Height * 3 * 108 / 1440);
                    
                    Collision4.CollisionRect = new Rectangle(CenteroftheUniverseX + GraphicsDevice.Viewport.Width * 5 * 336 / 3200,
                        CenteroftheUniverseY + GraphicsDevice.Viewport.Height * 3 * 478 / 1440,
                        GraphicsDevice.Viewport.Width * 5 * 205 / 3200, GraphicsDevice.Viewport.Height * 3 * 455 / 1440);

                    Collision5.CollisionRect = new Rectangle(CenteroftheUniverseX + GraphicsDevice.Viewport.Width * 5 * 902 / 3200,
                        CenteroftheUniverseY + GraphicsDevice.Viewport.Height * 3 * 554 / 1440,
                        GraphicsDevice.Viewport.Width * 5 * 191 / 3200, GraphicsDevice.Viewport.Height * 3 * 382 / 1440);

                    Collision2.CollisionRect = new Rectangle(CenteroftheUniverseX + GraphicsDevice.Viewport.Width * 5 * 265 / 3200,
                        CenteroftheUniverseY + GraphicsDevice.Viewport.Height * 3 * 361 / 1440,
                        GraphicsDevice.Viewport.Width * 5 * 185 / 3200, GraphicsDevice.Viewport.Height * 3 * 79 / 1440);

                    Collision3.CollisionRect = new Rectangle(CenteroftheUniverseX + GraphicsDevice.Viewport.Width * 5 * 976 / 3200,
                        CenteroftheUniverseY + GraphicsDevice.Viewport.Height * 3 * 347 / 1440,
                        GraphicsDevice.Viewport.Width * 5 * 131 / 3200, GraphicsDevice.Viewport.Height * 3 * 83 / 1440);

                    realGameMovementupdate();

                    if (Collision1.CollisionRect.Intersects(cursorRect))
                    {
                        forwardText();
                        if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                        || keys.IsKeyDown(Keys.A))
                        {
                            CangoForward = false;
                            CanContinue = false;
                            CanEnter = false;
                            CenteroftheUniverseX = -2161;
                            CenteroftheUniverseY = -450;
                            global_gameState = SCENE3;
                        }
                    }

                    if (Collision4.CollisionRect.Intersects(cursorRect))
                    {
                        forwardText();
                        if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                        || keys.IsKeyDown(Keys.A))
                        {
                            CangoForward = false;
                            CanContinue = false;
                            CanEnter = false;
                            CenteroftheUniverseX = -765;
                            CenteroftheUniverseY = -393;
                            HasAttacked = false;
                            global_gameState = SCENE6;
                        }
                    }

                    if (Collision5.CollisionRect.Intersects(cursorRect))
                    {
                        forwardText();
                        if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                        || keys.IsKeyDown(Keys.A))
                        {
                            CenteroftheUniverseX = -411;
                            CenteroftheUniverseY = -431;
                            HasAttacked = false;
                            global_gameState = SCENE8;
                        }
                    }

                    if (Collision2.CollisionRect.Intersects(cursorRect))
                    {
                        output = "Left Click/A: Examine";
                        if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                        || keys.IsKeyDown(Keys.A))
                        {
                            output = "This appears to be some sort of 'Library'.... Who knew?";
                        }
                    }

                    if (Collision3.CollisionRect.Intersects(cursorRect))
                    {
                        output = "Left Click/A: Examine";
                        if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                        || keys.IsKeyDown(Keys.A))
                        {
                            output = "That sounds kind of creepy....";
                        }
                    }

                    if (HasKnocked == true && HasAttacked == false)
                    {
                        if (HasGenerated == false)
                        {
                            randomgenerator();
                            HasGenerated = true;
                        }

                        if (randomWolfAttack == 2)
                            wolfAttackA();
                    }

                    break;

                case SCENE6:

                    if (MediaPlayer.State != MediaState.Playing)
                        MediaPlayer.Play(InGameSong);

                    Scene6.BackgroundRect = new Rectangle(CenteroftheUniverseX, CenteroftheUniverseY,
                            GraphicsDevice.Viewport.Width * 5, GraphicsDevice.Viewport.Height * 3);

                    Collision1.CollisionRect = new Rectangle(CenteroftheUniverseX + GraphicsDevice.Viewport.Width * 5 * 901 / 3200,
                        CenteroftheUniverseY + GraphicsDevice.Viewport.Height * 3 * 581 / 1440,
                        GraphicsDevice.Viewport.Width * 5 * 91 / 3200, GraphicsDevice.Viewport.Height * 3 * 101 / 1440);
                    
                    Collision2.CollisionRect = new Rectangle(CenteroftheUniverseX + GraphicsDevice.Viewport.Width * 5 * 463 / 3200,
                        CenteroftheUniverseY + GraphicsDevice.Viewport.Height * 3 * 395 / 1440,
                        GraphicsDevice.Viewport.Width * 5 * 43 / 3200, GraphicsDevice.Viewport.Height * 3 * 121 / 1440);

                    Collision3.CollisionRect = new Rectangle(CenteroftheUniverseX + GraphicsDevice.Viewport.Width * 5 * 2077 / 3200,
                        CenteroftheUniverseY + GraphicsDevice.Viewport.Height * 3 * 407 / 1440,
                        GraphicsDevice.Viewport.Width * 5 * 333 / 3200, GraphicsDevice.Viewport.Height * 3 * 549 / 1440);

                    realGameMovementupdate();

                    if (Collision1.CollisionRect.Intersects(cursorRect))
                    {
                        output = "Looks like the main desk..... Left Click/A: Forward";
                        if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                        || keys.IsKeyDown(Keys.A))
                        {
                                CenteroftheUniverseX = -1575;
                                CenteroftheUniverseY = -260;
                                HasAttacked = false;
                                global_gameState = SCENE7;
                        }
                    }

                    else if (Collision2.CollisionRect.Intersects(cursorRect))
                    {
                        itemText();
                        if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                        || keys.IsKeyDown(Keys.A))
                        {
                            if (HasKnocked == false)
                                output = "I don't need this!";

                            else if (HasKnocked == true)
                            {
                                if (BibleState2 == 0)
                                    BibleState2 = 1;
                            }
                        }
                    }

                    else if (BibleState2 == 1)
                        BibleState2 = 2;

                    else if (Collision3.CollisionRect.Intersects(cursorRect))
                    {
                        forwardText();
                        if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                        || keys.IsKeyDown(Keys.A))
                        {
                            CangoForward = false;
                            CanContinue = false;
                            CanEnter = false;
                            CenteroftheUniverseX = -1677;
                            CenteroftheUniverseY = -500;
                            HasAttacked = false;
                            global_gameState = SCENE5;
                        }
                    }

                    else if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                        || keys.IsKeyDown(Keys.A))
                    {
                        output = "Are these supposed to be.... BOOKS on these shelves? I hope no one's getting paid to draw this....";
                    }

                    if (HasKnocked == true && HasAttacked == false)
                    {
                        if (HasGenerated == false)
                        {
                            randomgenerator();
                            HasGenerated = true;
                        }

                        if (randomWolfAttack == 2)
                            wolfAttackA();
                    }

                    break;

                case SCENE7:

                    if (MediaPlayer.State != MediaState.Playing)
                        MediaPlayer.Play(InGameSong);

                    Scene7.BackgroundRect = new Rectangle(CenteroftheUniverseX, CenteroftheUniverseY,
                            GraphicsDevice.Viewport.Width * 5, GraphicsDevice.Viewport.Height * 3);

                    Scene7Character.BackgroundRect = new Rectangle(CenteroftheUniverseX + GraphicsDevice.Viewport.Width * 5 * 1787 / 3200,
                        CenteroftheUniverseY + GraphicsDevice.Viewport.Height * 3 * 332 / 1440,
                        GraphicsDevice.Viewport.Width * 5 * 161 / 3200, GraphicsDevice.Viewport.Height * 3 * 211 / 1440);

                    Collision1.CollisionRect = new Rectangle(CenteroftheUniverseX + GraphicsDevice.Viewport.Width * 5 * 335 / 3200,
                        CenteroftheUniverseY + GraphicsDevice.Viewport.Height * 3 * 578 / 1440,
                        GraphicsDevice.Viewport.Width * 5 * 53 / 3200, GraphicsDevice.Viewport.Height * 3 * 105 / 1440);

                    realGameMovementupdate();

                    if (Scene7Character.BackgroundRect.Intersects(cursorRect))
                    {
                        talkText();
                        if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                        || keys.IsKeyDown(Keys.A))
                        {
                            output = "Librarian: 'I'm not helping you find ANYTHING.'";
                        }
                    }

                    if (Collision1.CollisionRect.Intersects(cursorRect))
                    {
                        forwardText();
                        if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                        || keys.IsKeyDown(Keys.A))
                        {
                            CangoForward = false;
                            CanContinue = false;
                            CanEnter = false;
                            CenteroftheUniverseX = -2045;
                            CenteroftheUniverseY = -450;
                            HasAttacked = false;
                            global_gameState = SCENE6;
                        }
                    }

                    break;

                case SCENE8:

                    if (MediaPlayer.State != MediaState.Playing)
                        MediaPlayer.Play(InGameSong);

                    Scene8.BackgroundRect = new Rectangle(CenteroftheUniverseX, CenteroftheUniverseY,
                            GraphicsDevice.Viewport.Width * 5, GraphicsDevice.Viewport.Height * 3);

                    Collision1.CollisionRect = new Rectangle(CenteroftheUniverseX + GraphicsDevice.Viewport.Width * 5 * 1877 / 3200,
                        CenteroftheUniverseY + GraphicsDevice.Viewport.Height * 3 * 501 / 1440,
                        GraphicsDevice.Viewport.Width * 5 * 249 / 3200, GraphicsDevice.Viewport.Height * 3 * 433 / 1440);

                    Collision2.CollisionRect = new Rectangle(CenteroftheUniverseX + GraphicsDevice.Viewport.Width * 5 * 1061 / 3200,
                        CenteroftheUniverseY + GraphicsDevice.Viewport.Height * 3 * 893 / 1440,
                        GraphicsDevice.Viewport.Width * 5 * 41 / 3200, GraphicsDevice.Viewport.Height * 3 * 57 / 1440);

                    Collision3.CollisionRect = new Rectangle(CenteroftheUniverseX + GraphicsDevice.Viewport.Width * 5 * 525 / 3200,
                        CenteroftheUniverseY + GraphicsDevice.Viewport.Height * 3 * 523 / 1440,
                        GraphicsDevice.Viewport.Width * 5 * 65 / 3200, GraphicsDevice.Viewport.Height * 3 * 131 / 1440);

                    realGameMovementupdate();

                    if (Collision1.CollisionRect.Intersects(cursorRect))
                    {
                        forwardText();
                        if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                        || keys.IsKeyDown(Keys.A))
                        {
                            CangoForward = false;
                            CanContinue = false;
                            CanEnter = false;
                            CenteroftheUniverseX = -1677;
                            CenteroftheUniverseY = -500;
                            HasAttacked = false;
                            global_gameState = SCENE5;
                        }
                    }

                    if (Collision2.CollisionRect.Intersects(cursorRect))
                    {
                        itemText();
                        if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                        || keys.IsKeyDown(Keys.A))
                        {
                            if (HasKnocked == false)
                                output = "I don't need this!";

                            else if (HasKnocked == true)
                            {
                                if (BibleState3 == 0)
                                    BibleState3 = 1;
                            }
                        }
                    }

                    else if (BibleState3 == 1)
                        BibleState3 = 2;

                    if (Collision3.CollisionRect.Intersects(cursorRect))
                    {
                        forwardText();
                        if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                        || keys.IsKeyDown(Keys.A))
                        {
                            HasAttacked = false;
                            global_gameState = SCENE9;
                            CangoForward = false;
                            CanContinue = false;
                        }
                    }

                    if (HasKnocked == true && HasAttacked == false)
                    {
                        if (HasGenerated == false)
                        {
                            randomgenerator();
                            HasGenerated = true;
                        }

                        if (randomWolfAttack == 2)
                            wolfAttackA();
                    }

                    break;

                case SCENE9:

                    if (MediaPlayer.State != MediaState.Playing)
                        MediaPlayer.Play(InGameSong);

                     output = " ";

            curMousePosX = ms.X;
            ms = Mouse.GetState();
            curMousePosY = ms.Y;
                    Mouse.SetPosition(cursorX, cursorY);

                    Scene9A.BackgroundRect = new Rectangle(1, 1,
                           GraphicsDevice.Viewport.Width, GraphicsDevice.Viewport.Height);

                    Scene9B.BackgroundRect = new Rectangle(1, 1,
                           GraphicsDevice.Viewport.Width, GraphicsDevice.Viewport.Height);

                    if (HasKnocked == false)
                    {
                        output = "Right Click/B: Knock        Shift/X: Back Away";
                        if (ms.RightButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.B == ButtonState.Pressed
                        || keys.IsKeyDown(Keys.B) && PlayerisPunching == false)
                        {
                            PlayerisPunching = true;
                        }
                    }

                    if (HasKnocked == true)
                    {
                        output = "Left Click/A: Forward        Shift/X: Back Away";
                        if (ms.RightButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.B == ButtonState.Pressed
                        || keys.IsKeyDown(Keys.B))
                        {
                            output = "No..... I'm not knocking on that thing again.";
                        }

                        if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                        || keys.IsKeyDown(Keys.A))
                        {
                            output = "There appears to be three locks on this door. Presumably, I need three keys.";
                        }
                    }

                    if (GamePad.GetState(PlayerIndex.One).Buttons.X == ButtonState.Pressed
                        || keys.IsKeyDown(Keys.LeftShift) || keys.IsKeyDown(Keys.RightShift) || keys.IsKeyDown(Keys.X))
                    {
                        global_gameState = SCENE8;
                    }

                    if (keys.IsKeyDown(Keys.Space) || GamePad.GetState(PlayerIndex.One).Buttons.Y == ButtonState.Pressed)
                    {
                        PlayerisSmoking = true;
                    }

                    else
                    {
                        PlayerisSmoking = false;
                    }

                    if (HasKnocked == true)
                    {
                        videoTexture2 = null;
                        if (player2.State != MediaState.Stopped)
                        {
                            output = "";
                            videoTexture2 = player2.GetTexture();
                            global_gameState = SCENE9;
                        }

                        if (BibleState1 > 0 && BibleState2 > 0 && BibleState3 > 0)
                        {
                            CanContinue = true;
                            BibleState1 = 0;
                            player3.Play(cutscene3);
                        }

                        if (CanContinue == true)
                        {
                            videoTexture3 = null;
                            if (player3.State != MediaState.Stopped)
                            {
                                output = "";
                                videoTexture3 = player3.GetTexture();
                                global_gameState = SCENE9;
                            }

                            else
                            {
                                player4.Play(credits);
                                global_gameState = CREDITS_SCENE;
                                BibleState1 = 0;
                                BibleState2 = 0;
                                BibleState3 = 0;
                                CangoForward = false;
                                CanContinue = false;
                                HasKnocked = false;
                                WolfAttackAOver = false;
                                HasGenerated = false;
                                HasAttacked = false;
                            }
                        }
                    }

                    break;

                case PAUSED:

                    curMousePosX = ms.X;
            ms = Mouse.GetState();
            curMousePosY = ms.Y;
            Mouse.SetPosition(cursorX, cursorY);

                    if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                        || keys.IsKeyDown(Keys.A))
                    {
                        global_gameState = LastState;
                    }

                    if (ms.RightButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.B == ButtonState.Pressed
                        || keys.IsKeyDown(Keys.B))
                    {
                        CenteroftheUniverseX = -430;
                        CenteroftheUniverseY = -460;
                        global_gameState = MAIN_MENU;
                    }

                    if (GamePad.GetState(PlayerIndex.One).Buttons.X == ButtonState.Pressed
                        || keys.IsKeyDown(Keys.LeftShift) || keys.IsKeyDown(Keys.RightShift) || keys.IsKeyDown(Keys.X))
                    {
                        this.Exit();
                    }

                    break;

                case CREDITS_SCENE:

                    creditsTexture = null;
                    if (player4.State != MediaState.Stopped)
                    {
                        output = "";
                        creditsTexture = player4.GetTexture();
                    }

                    else
                    {
                        CenteroftheUniverseX = -430;
                        CenteroftheUniverseY = -460;
                        global_gameState = MAIN_MENU;
                    }

                    break;
            }

            // TODO: Add your update logic here
            

            base.Update(gameTime);
        }

        /// <summary>
        /// This is called when the game should draw itself.
        /// </summary>
        /// <param name="gameTime">Provides a snapshot of timing values.</param>
        protected override void Draw(GameTime gameTime)
        {
            GraphicsDevice.Clear(Color.Black);

            // TODO: Add your drawing code here
            spriteBatch.Begin();

            switch (global_gameState)
            {
                case MAIN_MENU:

                    spriteBatch.Draw(Menu.BackgroundTexture, Menu.BackgroundRect, Color.White);
                    spriteBatch.Draw(Collision1.CollisionTexture, Collision1.CollisionRect, Color.White);
                    spriteBatch.Draw(Collision2.CollisionTexture, Collision2.CollisionRect, Color.White);

                    if(ShowNotice == true)
                        spriteBatch.Draw(Notice.BackgroundTexture, Notice.BackgroundRect, Color.White);

                    realGameMovementdraw();

                    if(showdocumentation == true)
                        if (DocumentationTex != null)
                        {
                            spriteBatch.Draw(DocumentationTex, new Rectangle(0, 0,
                                GraphicsDevice.Viewport.Width, GraphicsDevice.Viewport.Height), Color.White);
                        }

                    break;

                case MINI_GAME1:

                    if (StirLeft == false)
                    {
                        spriteBatch.Draw(miniGame1.BackgroundTexture, miniGame1.BackgroundRect, Color.White);
                    }

                    if (StirLeft == true)
                    {
                        spriteBatch.Draw(miniGame2.BackgroundTexture, miniGame2.BackgroundRect, Color.White);
                    }

                    break;

                case MINI_GAME2:

                    if (StirLeft == true)
                        spriteBatch.Draw(miniGame3.BackgroundTexture, miniGame3.BackgroundRect, Color.White);

                    if(StirLeft == false)
                        spriteBatch.Draw(miniGame4.BackgroundTexture, miniGame4.BackgroundRect, Color.White);

                    break;

                case MINI_GAME3:

                    if (VideoisOver == false)
                    {
                        if (videoTexture5 != null)
                        {
                            spriteBatch.Draw(videoTexture5, new Rectangle(0, 0,
                                GraphicsDevice.Viewport.Width, GraphicsDevice.Viewport.Height), Color.White);
                        }
                    }

                    if(VideoisOver == true)
                    fakeGameMovementdraw();

                    break;

                case MINI_GAME4:

                    if (VideoisOver == false)
                    {
                        if (videoTexture6 != null)
                        {
                            spriteBatch.Draw(videoTexture6, new Rectangle(0, 0,
                                GraphicsDevice.Viewport.Width, GraphicsDevice.Viewport.Height), Color.White);
                        }
                    }

                    else if (VideoisOver == true)
                    {
                        spriteBatch.Draw(miniGameAfter.BackgroundTexture, miniGameAfter.BackgroundRect, Color.White);
                    }

                    break;

                case SCENE1:

                    spriteBatch.Draw(Scene1.BackgroundTexture, Scene1.BackgroundRect, Color.White);
                    spriteBatch.Draw(Collision1.CollisionTexture, Collision1.CollisionRect, Color.White);
                    spriteBatch.Draw(Collision2.CollisionTexture, Collision2.CollisionRect, Color.White);

                    if (videoTexture1 != null)
                    {
                        spriteBatch.Draw(videoTexture1, new Rectangle(0, 0, 
                            GraphicsDevice.Viewport.Width, GraphicsDevice.Viewport.Height), Color.White);                       
                    }


                    realGameMovementdraw();
                    break;

                case SCENE2:

                    if (CanContinue == false)
                    {
                        spriteBatch.Draw(Scene2.BackgroundTexture, Scene2.BackgroundRect, Color.White);
                    }

                    else if (CanContinue == true && CangoForward == false)
                    {
                        spriteBatch.Draw(Scene2Fall2.BackgroundTexture, Scene2Fall2.BackgroundRect, Color.White);
                    }

                    realGameMovementdraw();

                    if (CangoForward == true)
                    {
                        spriteBatch.Draw(Scene3.BackgroundTexture, Scene3.BackgroundRect, Color.White);
                        realGameMovementdraw();

                        spriteBatch.Draw(BlackOut.BackgroundTexture, BlackOut.BackgroundRect, Color.Red * BlackOutAlpha);
                    }
                    
                    break;

                case SCENE3:

                    //if(FadeColor.A < 255)
                    //{
                    //    FadeColor.A += 1;
                    //}
                    spriteBatch.Draw(Scene3.BackgroundTexture, Scene3.BackgroundRect, Color.White);
                    spriteBatch.Draw(Collision1.CollisionTexture, Collision1.CollisionRect, Color.White);
                    spriteBatch.Draw(Collision2.CollisionTexture, Collision2.CollisionRect, Color.White);
                    spriteBatch.Draw(Collision3.CollisionTexture, Collision3.CollisionRect, Color.White);
                    realGameMovementdraw();
                    //spriteBatch.Draw(BlackOut.BackgroundTexture, BlackOut.BackgroundRect, Color.White*BlackOutAlpha);

                    break;

                case SCENE4:

                    spriteBatch.Draw(Scene4.BackgroundTexture, Scene4.BackgroundRect, Color.White);

                    spriteBatch.Draw(Scene4Man.BackgroundTexture, Scene4Man.BackgroundRect, Color.White);

                    spriteBatch.Draw(Collision1.CollisionTexture, Collision1.CollisionRect, Color.White);
                    spriteBatch.Draw(Collision2.CollisionTexture, Collision2.CollisionRect, Color.White);

                    if (Collision1.CollisionRect.Intersects(cursorRect))
                    {
                        if (BibleState1 == 1)
                        {
                            output = "Look, a key! I'll just take that.....";
                            spriteBatch.Draw(OpenBible1A.BackgroundTexture, OpenBible1A.BackgroundRect, Color.White);
                        }

                        if (BibleState1 == 2)
                        {
                            output = "I've already taken the key.";
                            spriteBatch.Draw(OpenBible1B.BackgroundTexture, OpenBible1A.BackgroundRect, Color.White);
                        }

                    }

                    realGameMovementdraw();
                    break;

                case SCENE5:

                    spriteBatch.Draw(Scene5.BackgroundTexture, Scene5.BackgroundRect, Color.White);
                    spriteBatch.Draw(Collision1.CollisionTexture, Collision1.CollisionRect, Color.White);
                    spriteBatch.Draw(Collision2.CollisionTexture, Collision2.CollisionRect, Color.White);
                    spriteBatch.Draw(Collision3.CollisionTexture, Collision3.CollisionRect, Color.White);
                    spriteBatch.Draw(Collision4.CollisionTexture, Collision4.CollisionRect, Color.White);
                    spriteBatch.Draw(Collision5.CollisionTexture, Collision5.CollisionRect, Color.White);

                    if (HasKnocked == true)
                    {
                        spriteBatch.Draw(WolfAttack1.BackgroundTexture, WolfAttack1.BackgroundRect, Color.White);
                        if (WolfAttackAOver == true)
                        {
                            spriteBatch.Draw(WolfAttack2.BackgroundTexture, WolfAttack2.BackgroundRect, Color.White * Wolfattack2alpha);
                            spriteBatch.Draw(BlackOut.BackgroundTexture, BlackOut.BackgroundRect, Color.Red * BlackOutAlpha);
                        }
                    }

                    realGameMovementdraw();
                    break;

                case SCENE6:

                    spriteBatch.Draw(Scene6.BackgroundTexture, Scene6.BackgroundRect, Color.White);
                    spriteBatch.Draw(Collision1.CollisionTexture, Collision1.CollisionRect, Color.White);
                    spriteBatch.Draw(Collision2.CollisionTexture, Collision2.CollisionRect, Color.White);
                    spriteBatch.Draw(Collision3.CollisionTexture, Collision3.CollisionRect, Color.White);

                    if (Collision2.CollisionRect.Intersects(cursorRect))
                    {
                        if (BibleState2 == 1)
                        {
                            output = "Look, a key! I'll just take that.....";
                            spriteBatch.Draw(OpenBible2A.BackgroundTexture, OpenBible1A.BackgroundRect, Color.White);
                        }

                        if (BibleState2 == 2)
                        {
                            output = "I've already taken the key.";
                            spriteBatch.Draw(OpenBible2B.BackgroundTexture, OpenBible1A.BackgroundRect, Color.White);
                        }

                    }

                    if (HasKnocked == true)
                    {
                        spriteBatch.Draw(WolfAttack1.BackgroundTexture, WolfAttack1.BackgroundRect, Color.White);
                        if (WolfAttackAOver == true)
                        {
                            spriteBatch.Draw(WolfAttack2.BackgroundTexture, WolfAttack2.BackgroundRect, Color.White * Wolfattack2alpha);
                            spriteBatch.Draw(BlackOut.BackgroundTexture, BlackOut.BackgroundRect, Color.Red * BlackOutAlpha);
                        }
                    }

                    realGameMovementdraw();
                    break;

                case SCENE7:

                    spriteBatch.Draw(Scene7.BackgroundTexture, Scene7.BackgroundRect, Color.White);

                    spriteBatch.Draw(Scene7Character.BackgroundTexture, Scene7Character.BackgroundRect, Color.White);

                    spriteBatch.Draw(Collision1.CollisionTexture, Collision1.CollisionRect, Color.White);

                    realGameMovementdraw();
                    break;

                case SCENE8:

                    spriteBatch.Draw(Scene8.BackgroundTexture, Scene8.BackgroundRect, Color.White);
                    spriteBatch.Draw(Collision1.CollisionTexture, Collision1.CollisionRect, Color.White);
                    spriteBatch.Draw(Collision2.CollisionTexture, Collision2.CollisionRect, Color.White);

                    if (Collision2.CollisionRect.Intersects(cursorRect))
                    {
                        if (BibleState3 == 1)
                        {
                            output = "Look, a key! I'll just take that.....";
                            spriteBatch.Draw(OpenBible3A.BackgroundTexture, OpenBible1A.BackgroundRect, Color.White);
                        }

                        if (BibleState3 == 2)
                        {
                            output = "I've already taken the key.";
                            spriteBatch.Draw(OpenBible3B.BackgroundTexture, OpenBible1A.BackgroundRect, Color.White);
                        }

                    }

                    if (HasKnocked == true)
                    {
                        spriteBatch.Draw(WolfAttack1.BackgroundTexture, WolfAttack1.BackgroundRect, Color.White);
                        if (WolfAttackAOver == true)
                        {
                            spriteBatch.Draw(WolfAttack2.BackgroundTexture, WolfAttack2.BackgroundRect, Color.White * Wolfattack2alpha);
                            spriteBatch.Draw(BlackOut.BackgroundTexture, BlackOut.BackgroundRect, Color.Red * BlackOutAlpha);
                        }
                    }

                    realGameMovementdraw();
                    break;

                case SCENE9:

                    if (HasKnocked == false)
                    {
                        spriteBatch.Draw(Scene9A.BackgroundTexture, Scene9A.BackgroundRect, Color.White);

                if (PlayerisPunching == true)
                {

                    if (Punch.BackgroundRect.Y >=  GraphicsDevice.Viewport.Height -232
                        && IsgoingDown == false)
                    {
                        Punch.BackgroundRect.Y -= 4;
                    }

                    else if (IsgoingDown == false)
                    {
                            spriteBatch.Draw(Scene9B.BackgroundTexture, Scene9B.BackgroundRect, Color.White);
                            output = "Uh.... Never mind...";
                            spriteBatch.DrawString(Font1, output, Font1Pos, Color.White);

                        IsgoingDown = true;
                    }

                    else if (Punch.BackgroundRect.Y < GraphicsDevice.Viewport.Height && IsgoingDown == true)
                    {
                        spriteBatch.Draw(Scene9B.BackgroundTexture, Scene9B.BackgroundRect, Color.White);
                        output = "Uh.... Never mind...";
                        spriteBatch.DrawString(Font1, output, Font1Pos, Color.White);
                        Punch.BackgroundRect.Y += 4;
                    }

                    if(Punch.BackgroundRect.Y == GraphicsDevice.Viewport.Height)
                    {
                        IsgoingDown = false;
                        PlayerisPunching = false;
                        HasKnocked = true;
                        player2.Play(cutscene2);
                    }

                }
                    }

                    else if (HasKnocked == true)
                    {
                        spriteBatch.Draw(Scene9B.BackgroundTexture, Scene9B.BackgroundRect, Color.White);
                        if (videoTexture2 != null)
                        {
                            spriteBatch.Draw(videoTexture2, new Rectangle(0, 0,
                                GraphicsDevice.Viewport.Width, GraphicsDevice.Viewport.Height), Color.White);
                        }

                        if (CanContinue == true)
                        {
                            if (videoTexture3 != null)
                            {
                                spriteBatch.Draw(videoTexture3, new Rectangle(0, 0,
                                    GraphicsDevice.Viewport.Width, GraphicsDevice.Viewport.Height), Color.White);
                            }
                        }
                    }

                    if (PlayerisSmoking == true)
                    {
                        SmokingTimer++;

                        if (SmokingTimer <= 30)
                        {
                            spriteBatch.Draw(pipeTexture2, pipeRect1, Color.White);
                        }

                        else if (SmokingTimer >= 31 && SmokingTimer < 59)
                        {
                            spriteBatch.Draw(pipeTexture1, pipeRect1, Color.White);
                        }

                        else
                        {
                            SmokingTimer = 0;
                        }
                    }

                    if(BibleState1 > 0)
                        spriteBatch.Draw(Key1.BackgroundTexture, Key1.BackgroundRect, Color.White);

                    if(BibleState2 > 0)
                        spriteBatch.Draw(Key2.BackgroundTexture, Key2.BackgroundRect, Color.White);

                    if(BibleState3 > 0)
                        spriteBatch.Draw(Key3.BackgroundTexture, Key3.BackgroundRect, Color.White);

                    spriteBatch.Draw(Punch.BackgroundTexture, Punch.BackgroundRect, Color.White);
                    spriteBatch.Draw(cursorTexture, cursorRect, Color.White);

                    if (HasKnocked == true)
                    {
                        if (CanContinue == true)
                        {
                            if (videoTexture3 != null)
                            {
                                spriteBatch.Draw(videoTexture3, new Rectangle(0, 0,
                                    GraphicsDevice.Viewport.Width, GraphicsDevice.Viewport.Height), Color.White);
                            }
                        }
                    }

                    break;

                case PAUSED:

                    spriteBatch.Draw(Paused.BackgroundTexture, Paused.BackgroundRect, Color.White);
                    break;

                case CREDITS_SCENE:

                    if (creditsTexture != null)
                    {
                        spriteBatch.Draw(creditsTexture, new Rectangle(0, 0,
                            GraphicsDevice.Viewport.Width, GraphicsDevice.Viewport.Height), Color.White);
                    }
                    break;
            }

            Vector2 FontOrigin = Font1.MeasureString(output) / 2;
            spriteBatch.DrawString(Font1, output, Font1Pos, Color.White);
            //, 0, FontOrigin, 1.0f, SpriteEffects.None, 0.5f

            spriteBatch.End();

            base.Draw(gameTime);
        }

        
        
        // Game Movement and interaction, Normal
        void realGameMovementupdate()
        {
            KeyboardState keys = Keyboard.GetState();
            GamePadState gamePad1 = GamePad.GetState(PlayerIndex.One);

            output = " ";

            curMousePosX = ms.X;
            ms = Mouse.GetState();
            curMousePosY = ms.Y;

            if (keys.IsKeyDown(Keys.Space) || GamePad.GetState(PlayerIndex.One).Buttons.Y == ButtonState.Pressed)
            {
                PlayerisSmoking = true;
            }

                else
            {
                PlayerisSmoking = false;
            }

            if (ms.RightButton == ButtonState.Pressed || 
                keys.IsKeyDown(Keys.B) || GamePad.GetState(PlayerIndex.One).Buttons.B == ButtonState.Pressed && PlayerisPunching == false)
            {
                PlayerisPunching = true;
            }

            if (CenteroftheUniverseX >= 2)
            {
                CenteroftheUniverseX = GraphicsDevice.Viewport.Width * -4;
            }

            if (curMousePosX > cursorX || gamePad1.ThumbSticks.Left.X > 0 || keys.IsKeyDown(Keys.Right))
            {
                CenteroftheUniverseX -= GraphicsDevice.Viewport.Width / 160;
            }

            if (curMousePosX < cursorX | gamePad1.ThumbSticks.Left.X < 0 || keys.IsKeyDown(Keys.Left))
            {
                CenteroftheUniverseX += GraphicsDevice.Viewport.Width / 160;
            }

            if (curMousePosY > cursorY || gamePad1.ThumbSticks.Left.Y < 0 || keys.IsKeyDown(Keys.Down))
            {
                CenteroftheUniverseY -= GraphicsDevice.Viewport.Width / 160;
            }

            if (curMousePosY < cursorY || gamePad1.ThumbSticks.Left.Y > 0 || keys.IsKeyDown(Keys.Up))
            {
                CenteroftheUniverseY += GraphicsDevice.Viewport.Width / 160;
            }

            if (CenteroftheUniverseX <= GraphicsDevice.Viewport.Width * -4)
            {
                CenteroftheUniverseX = 1;
            }

            if (CenteroftheUniverseY >= 1)
            {
                CenteroftheUniverseY = 1;
            }

            if (CenteroftheUniverseY <= GraphicsDevice.Viewport.Height * -2)
            {
                CenteroftheUniverseY = GraphicsDevice.Viewport.Height * -2;
            }

            Mouse.SetPosition(cursorX, cursorY);
        }

        void realGameMovementdraw()
        {
                if (PlayerisSmoking == true)
            {
                SmokingTimer++;

                if (SmokingTimer <= 30)
                {
                    spriteBatch.Draw(pipeTexture2, pipeRect1, Color.White);
                }

                else if (SmokingTimer >= 31 && SmokingTimer < 59)
                {
                    spriteBatch.Draw(pipeTexture1, pipeRect1, Color.White);
                }

                else
                {
                    SmokingTimer = 0;
                }
            }

                spriteBatch.Draw(Punch.BackgroundTexture, Punch.BackgroundRect, Color.White);

                if (PlayerisPunching == true)
                {
                    if (Punch.BackgroundRect.Y >=  GraphicsDevice.Viewport.Height -232
                        && IsgoingDown == false)
                    {
                        Punch.BackgroundRect.Y -= 4;
                    }

                    else if (IsgoingDown == false)
                    {
                        IsgoingDown = true;
                    }

                    else if (Punch.BackgroundRect.Y < GraphicsDevice.Viewport.Height && IsgoingDown == true)
                    {
                        Punch.BackgroundRect.Y += 4;
                    }

                    if(Punch.BackgroundRect.Y == GraphicsDevice.Viewport.Height)
                    {
                        hitsToWin++;
                        IsgoingDown = false;
                        PlayerisPunching = false;
                    }

                }


                spriteBatch.Draw(cursorTexture, cursorRect, Color.White);
        }

        //Main Menu
        void mainMenuupdates()
        {
           
        }

        void mainMenudraws()
        {
        }

        //Scene 1

        void fallingAnimation()
        {
            if (Collision2.CollisionRect.X > 1)
            {
                CenteroftheUniverseX = CenteroftheUniverseX - (GraphicsDevice.Viewport.Width / 160);
            }

             else if (Collision2.CollisionRect.X + Collision2.CollisionRect.Width < GraphicsDevice.Viewport.Width)
            {
                CenteroftheUniverseX += GraphicsDevice.Viewport.Width / 160;
            }

            else
            {
                if (Collision2.CollisionRect.Y < GraphicsDevice.Viewport.Height)
                {
                    CenteroftheUniverseY += GraphicsDevice.Viewport.Width / 160;
                }

                else
                {
                    for (int count = 0; count < 1; count++)
                    {
                        //CenteroftheUniverseY += GraphicsDevice.Viewport.Width / 160;
                    }

                    spriteBatch.Begin();
                    spriteBatch.Draw(Scene2.BackgroundTexture, Scene2.BackgroundRect, Color.White);
                    spriteBatch.Draw(cursorTexture, cursorRect, Color.White);
                    spriteBatch.End();

                    CangoForward = false;
                    CanContinue = false;
                global_gameState = SCENE2;
                }

            }
        }

        //Text
        void forwardText()
        {
            output = "Left Click/A: Forward";
        }

        void itemText()
        {
            output = "Left Click/A: Pick up";
        }

        void talkText()
        {
            output = "Left Click/A: Talk";
        }

        void attackText()
        {
            output = "Right Click/B: Punch";
        }

    void wolfAttackA()
{
    WolfAttack1.BackgroundRect = new Rectangle(CenteroftheUniverseX + (GraphicsDevice.Viewport.Width * 5 * randomWolfPosition / 3200), 
        CenteroftheUniverseY + GraphicsDevice.Viewport.Height,
                        GraphicsDevice.Viewport.Width, GraphicsDevice.Viewport.Height);

    //output = WolfAttack1.BackgroundRect.X.ToString();

    if (WolfAttack1.BackgroundRect.Intersects(cursorRect))
    {
        if (WolfAttack1.BackgroundRect.X > 5)
        {
            CenteroftheUniverseX -= GraphicsDevice.Viewport.Width / 160;
        }

        if (WolfAttack1.BackgroundRect.X < -5)
        {
            CenteroftheUniverseX += GraphicsDevice.Viewport.Width / 160;
        }

        if (WolfAttack1.BackgroundRect.Y > 5)
        {
            CenteroftheUniverseY -= GraphicsDevice.Viewport.Width / 160;
        }

        if (WolfAttack1.BackgroundRect.Y < -5)
        {
            CenteroftheUniverseY += GraphicsDevice.Viewport.Width / 160;
        }

        if (WolfAttack1.BackgroundRect.X >= -5 && 
            WolfAttack1.BackgroundRect.X <= 5 && WolfAttack1.BackgroundRect.Y <= 5 
            && WolfAttack1.BackgroundRect.Y >= -5 && WolfAttackAOver == false)
        {
            output = "Congrats.";
            CenteroftheUniverseY = 1;
            currentX = CenteroftheUniverseX;
            hitsToWin = 0;
            WolfAttackAOver = true;
            InGameSong3Instance.Play();
        }
    }

        if (WolfAttackAOver == true)
        {
            CenteroftheUniverseY = 1;
            CenteroftheUniverseX = currentX;
            WolfAttack2.BackgroundRect = new Rectangle(1, 1,
                        GraphicsDevice.Viewport.Width, GraphicsDevice.Viewport.Height);
            BlackOut.BackgroundRect = new Rectangle(1, 1,
                        GraphicsDevice.Viewport.Width, GraphicsDevice.Viewport.Height);
            attackText();

            if (hitsToWin >= 5)
            {
                InGameSong3Instance.Stop();

                if (Wolfattack2alpha > 0 && BlackOutAlpha > 0)
                {
                    Wolfattack2alpha-= 0.015f;
                    BlackOutAlpha -= 0.015f;
                }

                else
                {
                    randomWolfAttack = 1;
                    WolfAttackAOver = false;
                    BlackOutAlpha = 0f;
                    Wolfattack2alpha = 1f;
                    HasGenerated = false;
                    HasAttacked = true;
                }
            }

            else if (BlackOutAlpha < 1f)
            {
                GamePad.SetVibration(PlayerIndex.One, 0.5f, 0.5f);
                BlackOutAlpha += 0.0015f;
            }

            else if(BlackOutAlpha >= 1f)
            {
                GamePad.SetVibration(PlayerIndex.One, 1, 1);
                CangoForward = true;
                WolfAttackAOver = false;
                Wolfattack2alpha = 1f;
                HasGenerated = false;
                HasAttacked = false;
                BlackOutAlpha = 1f;
                InGameSong3Instance.Stop();
                global_gameState = SCENE2;
            }
        }

    //WolfAttack2.BackgroundRect = new Rectangle(CenteroftheUniverseX, CenteroftheUniverseY,
     //                   GraphicsDevice.Viewport.Width * 5, GraphicsDevice.Viewport.Height * 3);
}

    void randomgenerator()
    {
        random = new Random();
        randomWolfAttack = random.Next(1, 3);
        randomWolfPosition = random.Next(640, 1919);
        randomWolfAttack = 2;
    }


    void fakeGameMovementupdate()
    {
        KeyboardState keys = Keyboard.GetState();
        GamePadState gamePad1 = GamePad.GetState(PlayerIndex.One);

        output = "Left Click/A: Extinguish";

        curMousePosX = ms.X;
        ms = Mouse.GetState();
        curMousePosY = ms.Y;

        if (keys.IsKeyDown(Keys.Space) || GamePad.GetState(PlayerIndex.One).Buttons.Y == ButtonState.Pressed)
        {
            PlayerisSmoking = true;
        }

        else
        {
            PlayerisSmoking = false;
        }

        if (miniFire1.BackgroundRect.Top <= miniGame5.BackgroundRect.Top && miniFire1.BackgroundRect.Bottom >= miniGame5.BackgroundRect.Bottom)
        {
            VideoisOver = false;
            global_gameState = MINI_GAME4;
            player6.Play(cutscene6);
        }

        if (miniFire2.BackgroundRect.Top <= miniGame5.BackgroundRect.Top && miniFire2.BackgroundRect.Bottom >= miniGame5.BackgroundRect.Bottom)
        {
            VideoisOver = false;
            global_gameState = MINI_GAME4;
            player6.Play(cutscene6);
        }

        if (miniFire3.BackgroundRect.Top <= miniGame5.BackgroundRect.Top && miniFire3.BackgroundRect.Bottom >= miniGame5.BackgroundRect.Bottom)
        {
            VideoisOver = false;
            global_gameState = MINI_GAME4;
            player6.Play(cutscene6);
        }

        if (miniFire4.BackgroundRect.Top <= miniGame5.BackgroundRect.Top && miniFire4.BackgroundRect.Bottom >= miniGame5.BackgroundRect.Bottom)
        {
            VideoisOver = false;
            global_gameState = MINI_GAME4;
            player6.Play(cutscene6);
        }
        

        if (ms.LeftButton == ButtonState.Pressed || GamePad.GetState(PlayerIndex.One).Buttons.A == ButtonState.Pressed
                            || keys.IsKeyDown(Keys.A))
        {
            isExtinguishing = true;

            if (miniFire1.BackgroundRect.Intersects(miniCloud.BackgroundRect))
            {
                miniFire1.FireX = fireXgenerator(miniFire1.FireX);
                miniFire1.FireY = fireYgenerator(miniFire1.BackgroundRect.Height, miniFire1.FireY);
                miniFire1.BackgroundRect.Width = GraphicsDevice.Viewport.Width * 139 / 1280;
                miniFire1.BackgroundRect.Height = GraphicsDevice.Viewport.Height * 232 / 960;
                stirNum++;
            }

            if (miniFire2.BackgroundRect.Intersects(miniCloud.BackgroundRect))
            {
                miniFire2.FireX = fireXgenerator(miniFire2.FireX);
                miniFire2.FireY = fireYgenerator(miniFire2.BackgroundRect.Height, miniFire2.FireY);
                miniFire2.BackgroundRect.Width = GraphicsDevice.Viewport.Width * 139 / 1280;
                miniFire2.BackgroundRect.Height = GraphicsDevice.Viewport.Height * 232 / 960;
                stirNum++;
            }

            if (miniFire3.BackgroundRect.Intersects(miniCloud.BackgroundRect))
            {
                miniFire3.FireX = fireXgenerator(miniFire3.FireX);
                miniFire3.FireY = fireYgenerator(miniFire3.BackgroundRect.Height, miniFire3.FireY);
                miniFire3.BackgroundRect.Width = GraphicsDevice.Viewport.Width * 139 / 1280;
                miniFire3.BackgroundRect.Height = GraphicsDevice.Viewport.Height * 232 / 960;
                stirNum++;
            }

            if (miniFire4.BackgroundRect.Intersects(miniCloud.BackgroundRect))
            {
                miniFire4.FireX = fireXgenerator(miniFire4.FireX);
                miniFire4.FireY = fireYgenerator(miniFire4.BackgroundRect.Height, miniFire4.FireY);
                miniFire4.BackgroundRect.Width = GraphicsDevice.Viewport.Width * 139 / 1280;
                miniFire4.BackgroundRect.Height = GraphicsDevice.Viewport.Height * 232 / 960;
                stirNum++;
            }
        }

        else
        {
            isExtinguishing = false;
        }

        if (CenteroftheUniverseX >= 2)
        {
            CenteroftheUniverseX = GraphicsDevice.Viewport.Width * -3;
        }

        if (curMousePosX > cursorX || gamePad1.ThumbSticks.Left.X > 0 || keys.IsKeyDown(Keys.Right))
        {
            CenteroftheUniverseX -= GraphicsDevice.Viewport.Width / 160;
        }

        if (curMousePosX < cursorX | gamePad1.ThumbSticks.Left.X < 0 || keys.IsKeyDown(Keys.Left))
        {
            CenteroftheUniverseX += GraphicsDevice.Viewport.Width / 160;
        }

        if (CenteroftheUniverseX <= GraphicsDevice.Viewport.Width * -3)
        {
            CenteroftheUniverseX = 1;
        }

        miniFire1.BackgroundRect.Inflate(1, 1);
        miniFire2.BackgroundRect.Inflate(1, 1);
        miniFire3.BackgroundRect.Inflate(1, 1);
        miniFire4.BackgroundRect.Inflate(1, 1);

        Mouse.SetPosition(cursorX, cursorY);
    }

    void fakeGameMovementdraw()
    {
        spriteBatch.Draw(miniGame5.BackgroundTexture, miniGame5.BackgroundRect, Color.White);

        spriteBatch.Draw(miniFire1.BackgroundTexture, miniFire1.BackgroundRect, Color.White);
        spriteBatch.Draw(miniFire1.BackgroundTexture, miniFire2.BackgroundRect, Color.White);
        spriteBatch.Draw(miniFire1.BackgroundTexture, miniFire3.BackgroundRect, Color.White);
        spriteBatch.Draw(miniFire1.BackgroundTexture, miniFire4.BackgroundRect, Color.White);

        if(isExtinguishing == true)
        spriteBatch.Draw(miniCloud.BackgroundTexture, miniCloud.BackgroundRect, Color.White);

        spriteBatch.Draw(miniExtinguisher.BackgroundTexture, miniExtinguisher.BackgroundRect, Color.White);

        if (PlayerisSmoking == true)
        {
            SmokingTimer++;

            if (SmokingTimer <= 30)
            {
                spriteBatch.Draw(pipeTexture2, pipeRect1, Color.White);
            }

            else if (SmokingTimer >= 31 && SmokingTimer < 59)
            {
                spriteBatch.Draw(pipeTexture1, pipeRect1, Color.White);
            }

            else
            {
                SmokingTimer = 0;
            }
        }
    }


        int fireXgenerator(int fireX)
        {
            //random = new Random();
            fireX = random.Next(641, 1920);
            output = fireX.ToString();
            fireX = GraphicsDevice.Viewport.Width * 4 * fireX / 2560;

            return fireX;
        }

        int fireYgenerator(int fireHeight, int fireY)
        {
            //random = new Random();
            //fireY = random.Next(1, 248);
            fireY = GraphicsDevice.Viewport.Height * (191 - fireHeight / 2) / 480;
            //fireY = GraphicsDevice.Viewport.Height * 191 / 480;

            return fireY;
        }
}


}

