# ldrgen

**ldrgen** allows operators to generate shellcode loaders from a set of pre-defined templates and profiles with the aim of hopefully streamlining development and reducing the time it takes to get a new loader out the door.

## Getting Started
There are available binaries on the [releases](https://github.com/gatariee/ldrgen/releases) page, or you can build from source for the latest version.

### Usage
```bash
ldrgen profile --profile [path_to_profile] --shellcode [path_to_shellcode]
```
![hmm](https://i.gyazo.com/9b20c0e2699542268ed51aecba6eed2d.png)

### How does it work?
* Source files (`*.c`) or loaders are saved as templates in the `templates` directory, placeholders are also supported to be templated during generation.
* The template configuration is saved as [`config.yaml`](./templates/config.yaml) in the `templates` directory, this contains information about the loader, what source files it requires and whether any substitutions are required.
* Profiles are saved in the `profiles` directory, these can be used and customized to your personal preference to utilize the templates you've created.

#### Source
These are your loaders, most of the time these are the artifacts that get caught by AV. Put some effort into making some of these in your own time, and you'll have a nice collection of loaders to use!

[CreateThread_Xor.c](./templates/Source/CreateThread_Xor.c) is a good example of a simple loader that uses `CreateThread` to execute shellcode. It also has a `xorShellcode` function that will decrypt the shellcode before execution.
```c
...

    xorShellcode( shellcode, shellcode_size, "${KEY}" );

...
```

#### Templates
The ${KEY} placeholder is explicitly defined in the `config.yaml` file, and is used to substitute the key used to decrypt the shellcode. This is a simple example of how to use placeholders in your templates.
```yaml
  - token: "createthread_xor"
    key_required: true
    enc_type: "xor"
    method: "VirtualAlloc, xorShellcode, memcpy, CreateThread, WaitForSingleObject"
    files:
      - sourcePath: "Source/CreateThread_Xor.c"
        outputPath: "Main.c"
        substitutions: 
          key: "${KEY}"

      - sourcePath: "Source/Xor.c"
        outputPath: "Xor.c"
      
      - sourcePath: "Include/Xor.h"
        outputPath: "Xor.h"

      - sourcePath: "makefile"
        outputPath: "makefile"
```

#### Profiles
These are malleable configurations that can be used to generate a loader from a template.
```json
{
    "name": "Default Loader, but with XOR (CreateThread)",
    "author": "@gatari",
    "description": "A simple shellcode loader that uses CreateThread with XOR encrypted shellcode.",
    "template": {
        "path": "/opt/tools/ldrgen/templates/config.yaml",
        "token": "CreateThread_Xor",
        "enc_type": "xor",
        "substitutions": {
            "key": "as@&(!L@J#JKsn"
        }
    },
    "arch": "x64",
    "compile": {
        "automatic": true,
        "make": "make",
        "gcc": {
            "x64": "x86_64-w64-mingw32-gcc",
            "x86": "i686-w64-mingw32-gcc"
        },
        "strip": {
            "i686": "strip",
            "x86_64": "strip"
        }
    },
    "output_dir": "./createthreadxor"
}
```

### Releases
Standalone binaries are available, however `templates` and `profiles` will be ingested by `ldrgen`. The latest release should have these included.

### Building from Source
```bash
cd ldrgen
make build
```

### Design Principles
This tool is **not** designed to be turing complete by any means, it's designed to **streamline** the process of developing loaders- it is ultimately up to the operator to develop their own loaders and templates should they wish to do so.

`ldrgen` started as a personal project for encrypting beacon shellcode, then parsing it into a header file for use in a stageless loader. It was a pain to do this manually, so I decided to automate it- and it built from there.

### Contributing
If you'd like to contribute, feel free to open a PR or an issue. I'm always open to suggestions and improvements. However, before you make an issue, do remember that the objective of this tool is to streamline the process of developing loaders, not to be provide a cheat code for bypassing AV.


FCHashtbl.cmi
FCHashtbl.cmo
FCHashtbl.cmx
GSourceView.cmi
GSourceView.cmo
GSourceView.cmx
META.frama-c
abstract_interp.cmi
abstract_interp.cmo
abstract_interp.cmx
acsl_extension.cmi
acsl_extension.cmo
acsl_extension.cmx
alarms.cmi
alarms.cmo
alarms.cmx
allocates.cmi
allocates.cmo
allocates.cmx
alpha.cmi
alpha.cmo
alpha.cmx
analyses_manager.cmi
analyses_manager.cmo
analyses_manager.cmx
annotations.cmi
annotations.cmo
annotations.cmx
asm_contracts.cmi
asm_contracts.cmo
asm_contracts.cmx
ast.cmi
ast.cmo
ast.cmx
ast_diff.cmi
ast_diff.cmo
ast_diff.cmx
ast_info.cmi
ast_info.cmo
ast_info.cmx
bag.cmi
bag.cmo
bag.cmx
base.cmi
base.cmo
base.cmx
binary_cache.cmi
binary_cache.cmo
binary_cache.cmx
bit_utils.cmi
bit_utils.cmo
bit_utils.cmx
bitvector.cmi
bitvector.cmo
bitvector.cmx
book_manager.cmi
book_manager.cmo
book_manager.cmx
boot.cmi
boot.cmo
boot.cmx
cabs.cmi
cabs2cil.cmi
cabs2cil.cmo
cabs2cil.cmx
cabs_debug.cmi
cabs_debug.cmo
cabs_debug.cmx
cabshelper.cmi
cabshelper.cmo
cabshelper.cmx
cabsvisit.cmi
cabsvisit.cmo
cabsvisit.cmx
cfg.cmi
cfg.cmo
cfg.cmx
cil.cmi
cil.cmo
cil.cmx
cilE.cmi
cilE.cmo
cilE.cmx
cil_builder.cmi
cil_builder.cmo
cil_builder.cmx
cil_builtins.cmi
cil_builtins.cmo
cil_builtins.cmx
cil_const.cmi
cil_const.cmo
cil_const.cmx
cil_datatype.cmi
cil_datatype.cmo
cil_datatype.cmx
cil_descriptive_printer.cmi
cil_descriptive_printer.cmo
cil_descriptive_printer.cmx
cil_printer.cmi
cil_printer.cmo
cil_printer.cmx
cil_state_builder.cmi
cil_state_builder.cmo
cil_state_builder.cmx
cil_types.cmi
cil_types_debug.cmi
cil_types_debug.cmo
cil_types_debug.cmx
cilconfig.cmi
cilconfig.cmo
cilconfig.cmx
clexer.cmi
clexer.cmo
clexer.cmx
clone.cmi
clone.cmo
clone.cmx
cmdline.cmi
cmdline.cmo
cmdline.cmx
command.cmi
command.cmo
command.cmx
contract_special_float.cmi
contract_special_float.cmo
contract_special_float.cmx
cparser.cmi
cparser.cmo
cparser.cmx
cprint.cmi
cprint.cmo
cprint.cmx
cvalue.cmi
cvalue.cmo
cvalue.cmx
dataflow2.cmi
dataflow2.cmo
dataflow2.cmx
dataflows.cmi
dataflows.cmo
dataflows.cmx
datatype.cmi
datatype.cmo
datatype.cmx
db.cmi
db.cmo
db.cmx
descr.cmi
descr.cmo
descr.cmx
description.cmi
description.cmo
description.cmx
design.cmi
design.cmo
design.cmx
destructors.cmi
destructors.cmo
destructors.cmx
dgraph_helper.cmi
dgraph_helper.cmo
dgraph_helper.cmx
dllframa-c.so
dominators.cmi
dominators.cmo
dominators.cmx
dotgraph.cmi
dotgraph.cmo
dotgraph.cmx
dump_config.cmi
dump_config.cmo
dump_config.cmx
dynamic.cmi
dynamic.cmo
dynamic.cmx
e-acsl
emitter.cmi
emitter.cmo
emitter.cmx
errorloc.cmi
errorloc.cmo
errorloc.cmx
escape.cmi
escape.cmo
escape.cmx
eva_lattice_type.cmi
exn_flow.cmi
exn_flow.cmo
exn_flow.cmx
extlib.cmi
extlib.cmo
extlib.cmx
fc_config.cmi
fc_config.cmo
fc_config.cmx
fc_float.cmi
fc_float.cmo
fc_float.cmx
file.cmi
file.cmo
file.cmx
file_manager.cmi
file_manager.cmo
file_manager.cmx
filecheck.cmi
filecheck.cmo
filecheck.cmx
filepath.cmi
filepath.cmo
filepath.cmx
filetree.cmi
filetree.cmo
filetree.cmx
filter.cmi
filter.cmo
filter.cmx
float_interval.cmi
float_interval.cmo
float_interval.cmx
float_interval_sig.cmi
float_sig.cmi
floating_point.cmi
floating_point.cmo
floating_point.cmx
frama-c.a
frama-c.cma
frama-c.cmxa
frama_c_init.cmi
frama_c_init.cmo
frama_c_init.cmx
frontc.cmi
frontc.cmo
frontc.cmx
function_Froms.cmi
function_Froms.cmo
function_Froms.cmx
fval.cmi
fval.cmo
fval.cmx
ghost_accesses.cmi
ghost_accesses.cmo
ghost_accesses.cmx
ghost_cfg.cmi
ghost_cfg.cmo
ghost_cfg.cmx
globals.cmi
globals.cmo
globals.cmx
gtk_compat.cmi
gtk_compat.cmo
gtk_compat.cmx
gtk_form.cmi
gtk_form.cmo
gtk_form.cmx
gtk_helper.cmi
gtk_helper.cmo
gtk_helper.cmx
gui_parameters.cmi
gui_parameters.cmo
gui_parameters.cmx
gui_printers.cmi
gui_printers.cmo
gui_printers.cmx
help_manager.cmi
help_manager.cmo
help_manager.cmx
history.cmi
history.cmo
history.cmx
hook.cmi
hook.cmo
hook.cmx
hptmap.cmi
hptmap.cmo
hptmap.cmx
hptmap_sig.cmi
hptset.cmi
hptset.cmo
hptset.cmx
indexer.cmi
indexer.cmo
indexer.cmx
infer_annotations.cmi
infer_annotations.cmo
infer_annotations.cmx
inline.cmi
inline.cmo
inline.cmx
inout_type.cmi
inout_type.cmo
inout_type.cmx
int_Base.cmi
int_Base.cmo
int_Base.cmx
int_Intervals.cmi
int_Intervals.cmo
int_Intervals.cmx
int_Intervals_sig.cmi
int_interval.cmi
int_interval.cmo
int_interval.cmx
int_set.cmi
int_set.cmo
int_set.cmx
int_val.cmi
int_val.cmo
int_val.cmx
integer.cmi
integer.cmo
integer.cmx
interpreted_automata.cmi
interpreted_automata.cmo
interpreted_automata.cmx
ival.cmi
ival.cmo
ival.cmx
journal.cmi
journal.cmo
journal.cmx
json.cmi
json.cmo
json.cmx
json_compilation_database.cmi
json_compilation_database.cmo
json_compilation_database.cmx
kernel.cmi
kernel.cmo
kernel.cmx
kernel_function.cmi
kernel_function.cmo
kernel_function.cmx
lattice_bounds.cmi
lattice_bounds.cmo
lattice_bounds.cmx
lattice_messages.cmi
lattice_messages.cmo
lattice_messages.cmx
lattice_type.cmi
launcher.cmi
launcher.cmo
launcher.cmx
lexerhack.cmi
lexerhack.cmo
lexerhack.cmx
libframa-c.a
lmap.cmi
lmap.cmo
lmap.cmx
lmap_bitwise.cmi
lmap_bitwise.cmo
lmap_bitwise.cmx
lmap_sig.cmi
locations.cmi
locations.cmo
locations.cmx
log.cmi
log.cmo
log.cmx
logic_builtin.cmi
logic_builtin.cmo
logic_builtin.cmx
logic_const.cmi
logic_const.cmo
logic_const.cmx
logic_env.cmi
logic_env.cmo
logic_env.cmx
logic_interp.cmi
logic_interp.cmo
logic_interp.cmx
logic_lexer.cmi
logic_lexer.cmo
logic_lexer.cmx
logic_parser.cmi
logic_parser.cmo
logic_parser.cmx
logic_preprocess.cmi
logic_preprocess.cmo
