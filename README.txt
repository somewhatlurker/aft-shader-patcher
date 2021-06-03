AMD ARB Shader Patcher for Project DIVA Arcade Future Tone
================================================================================

AFT Shader Patcher is a tool designed to patch NVvp and NVfp shaders from
Project Diva Arcade Future Tone so they can work on AMD GPUs.

The ARB assembly patcher implemented within may also work with other games.
I've heard that it works for the original Project Diva Arcade games, but I have
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

Some technical information about the ARB patcher:

Pretty much everything is just based on regex rather than proper parsing.
Yes this is lazy and far from optimal, but it works well enough for the target
game so whatever.
Some of the patches use experimentally-determined things which by all accounts
probably shouldn't be supported (eg. IF in NV_vertex_program3)-- regardless,
they're usually the best solution known to me.

The ARB assembly patcher consists of a few stages.
1.  Find ATTRIBs, OUTPUTs, PARAMs and inline their assigned value to the shader
      code (replacing uses of their names). This only really seems to be needed
      for ATTRIB and OUTPUT (and only in vertex programs), but doing this helps
      when writing patches later (everything is consistend, with no need to
      care about the original shader devolopers' naming schemes).
      Array PARAMs are not inlined.
2.  Replace !!NVvpX.X and !!NVfpX.X headers with !!ARBvp1.0 and the appropriate
      OPTION (NV_vertex_program3 or NV_fragment_program2).
3.  Remove unused array PARAMs. (these still seem to count against the
      parameter limit otherwise, and on AMD it's lower than on NV).
4.  Add extra TEMP vars as necessary for NV instructions that are reimplemented.
5.  Perform all instruction substitutions.
6.  Apply game-specific instruction patches (for missing feature workarounds).
7.  Replace INT TEMP vars with TEMP.
8.  Run game-specific final post-filtering.
9.  Check for and remove some known-bad instructions if found remaining.
      (optionally alert user)
10. Remove all carriage return characters to ensure consistent newlines.

Some patch implementation notes:
- TEX sampling with pixel offsets is unsupported and just uses
    { 0.00078, 0.0014 } to approximate one pixel at 720p after patching.
    If the game being patched passes texture size to the shader somehow, this
    can be fixed in the post-filtering stage.
- Control flow (IF, RET, BRA) are quite likely to still have some issues.
    Particularly BRA seems to act up when multiple instructions share a target.
    (for now, game-specific code can be used to add multiple targets in the same
    location, however it stands out as something that could be fixed in the
    patcher)
- Currently known unsupported stuff includes: CVT, REP, and a lot of
    integer-based stuff (along with any unsupported extensions).
    Not all hope is lost, but we are not actively seeking solutions for these.

================================================================================

License is MIT license.