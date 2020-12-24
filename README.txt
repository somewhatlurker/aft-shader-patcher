AMD ARB Shader Patcher for Project DIVA Arcade Future Tone
================================================================================

AFT Shader Patcher is a tool designed to patch shaders from Project Diva Arcade
Future Tone so they can work on AMD GPUs.

The ARB Patcher implemented within may also work with other games.
I've heard that it also works for the original Project Diva Arcade, but I have
not personally verified this.

For now it's limited to Windows, due to use of xdelta3 in binary form.

================================================================================

Patching Instructions:
  1. Extract AFT Shader Patcher.
  2. Copy shader.farc into your extracted AFT Shader Patcher folder.
  3. Run "patch shaders". This may take several minutes.
  4. Output files will be created. Read below for details on how to use them.

================================================================================

Output Files:
  shader_patched.farc:
    The final patched shader file. This shouldn't be used for AFT and can be
    deleted, but it may be useful for other games.
  XXXXXXXX.vcdiff:
    A "delta patch" version of the new shader file. It can be used with the
    Novidia plugin for AFT (included with recent PD Loader versions) by copying
    it into your "plugins\Novidia Shaders" folder.
    After copying it, the original Nvidia shaders can be loaded by the game.

================================================================================

If you want to try using it with a game that doesn't use farc archives (or uses
an incompatible type), try using "AFT Shader Patcher\ARB Patcher\main.py" on its
own. It can be used standalone from a command line to attempt patching all files
in a directory. (eg. "python main.py -i shader_in -o shader_out")

================================================================================

ARB Patcher, the tool used to make modifications to shaders, was developed with
a lot of help from Nezarn, without whom AMD support would not be possible.

================================================================================

License is MIT license.