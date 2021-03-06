EXTS := OES_standard_derivatives \
	OES_texture_float \
	OES_texture_half_float \
	OES_vertex_array_object \
	WEBGL_lose_context \
	WEBGL_debug_renderer_info \
	WEBGL_debug_shaders \
	WEBGL_compressed_texture_s3tc \
	WEBGL_depth_texture \
	proposals/WEBGL_shared_resources \
	OES_element_index_uint \
	EXT_texture_filter_anisotropic \
	proposals/ANGLE_instanced_arrays \
	proposals/WEBGL_fbo_color_attachments \
	proposals/WEBGL_dynamic_texture \
	WEBGL_compressed_texture_pvrtc \
	WEBGL_compressed_texture_atc \
	proposals/WEBGL_multiple_render_targets \
	EXT_color_buffer_half_float \
	WEBGL_color_buffer_float \
	proposals/EXT_frag_depth \
	proposals/EXT_blend_minmax

EXTS := $(strip $(EXTS))

EXT_XMLS := $(addsuffix /extension.xml,$(EXTS))
EXT_IDXS := $(addsuffix /index.html,$(EXTS))

comma:=,
empty:=
space:=$(empty) $(empty)
EXT_LIST:=$(subst $(space),$(comma),$(EXTS))

# This sed substitute command builds a relative path back to the SVN root
# from an extension registry relative path
RE_BASEPATH:="s/[^\/]*/../g"

.PHONY: all clean

all: index.html index.atom $(EXT_IDXS) template/index.html

index.html: registry.xml registry.xsl registry.html rev_utils.xsl
	@echo "<!-- AUTOGENERATED FILE - DO NOT EDIT - SEE Makefile -->" > index.html
	xsltproc --stringparam basepath `echo "" | sed -e $(RE_BASEPATH)` \
	         --html registry.xsl registry.html >> index.html

index.atom: registry.xml revisions.atom atom.xsl
	xsltproc atom.xsl revisions.atom > index.atom

registry.xml: $(EXT_XMLS) summary.xsl summary.xml
	xsltproc --stringparam extensions $(EXT_LIST) \
	         summary.xsl summary.xml > registry.xml

%/index.html: %/extension.xml extension.xsl rev_utils.xsl standards.xsl
	xsltproc --stringparam basepath `echo $*/ | sed -e $(RE_BASEPATH)` \
	         extension.xsl $*/extension.xml > $*/index.html

clean:
	rm -f index.html index.atom registry.xml
	rm -f $(EXT_IDXS) template/index.html
