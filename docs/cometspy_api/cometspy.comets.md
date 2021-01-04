
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
  "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

<html xmlns="http://www.w3.org/1999/xhtml">
  <head>
    <meta http-equiv="X-UA-Compatible" content="IE=Edge" />
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>cometspy.comets module &#8212; cometspy 0.0.1 documentation</title>
    <link rel="stylesheet" href="_static/pygments.css" type="text/css" />
    <link rel="stylesheet" href="_static/alabaster.css" type="text/css" />
    <script id="documentation_options" data-url_root="./" src="_static/documentation_options.js"></script>
    <script src="_static/jquery.js"></script>
    <script src="_static/underscore.js"></script>
    <script src="_static/doctools.js"></script>
    <script src="_static/language_data.js"></script>
    <link rel="index" title="Index" href="genindex.html" />
    <link rel="search" title="Search" href="search.html" />
    <link rel="next" title="cometspy.layout module" href="cometspy.layout.html" />
    <link rel="prev" title="cometspy:" href="cometspy.html" />
   
  
  <meta name="viewport" content="width=device-width, initial-scale=0.9, maximum-scale=0.9" />

  </head><body>  

    <div class="document">
      <div class="documentwrapper">
        <div class="bodywrapper">
          <div class="body" role="main">
            
  <div class="section" id="module-cometspy.comets">
<span id="cometspy-comets-module"></span><h1>cometspy.comets module<a class="headerlink" href="#module-cometspy.comets" title="Permalink to this headline">¶</a></h1>
<p>The comets module runs COMETS simulations and stores output.</p>
<p>Generally, a comets object is created just before calling run(). Afterwards,
any saved data (e.g. total_biomass) can be accessed from the object.</p>
<dl class="py class">
<dt id="cometspy.comets.comets">
<em class="property">class </em><code class="descclassname">cometspy.comets.</code><code class="descname">comets</code><span class="sig-paren">(</span><em><span class="n">layout</span></em>, <em><span class="n">parameters</span></em>, <em><span class="n">relative_dir</span><span class="p">:</span> <span class="n">str</span> <span class="o">=</span> <span class="default_value">''</span></em><span class="sig-paren">)</span><a class="headerlink" href="#cometspy.comets.comets" title="Permalink to this definition">¶</a></dt>
<dd><p>Bases: <code class="xref py py-class docutils literal notranslate"><span class="pre">object</span></code></p>
<p>the main simulation object to run COMETS</p>
<p>The comets class stores a previously-made layout and params, and 
interacts with COMETS to run the simulation. It also stores any
generated output, which could include total_biomass, biomass, 
media, or fluxes, as set in the params object. std_out from the COMETS
simulation is saved in the attribute run_output and can be useful to 
examine to debug errors.</p>
<p>When creating a comets object, the optional relative_dir path is useful
when one is to run multiple simulations simultaneously, otherwise 
temporary files may overwrite each other.</p>
<table class="docutils field-list" frame="void" rules="none">
<col class="field-name" />
<col class="field-body" />
<tbody valign="top">
<tr class="field-odd field"><th class="field-name">Parameters:</th><td class="field-body"><dl class="first last docutils">
<dt><strong>layout</strong><span class="classifier">layout</span></dt><dd><p>a cometspy.layout containing models and media information</p>
</dd>
<dt><strong>parameters</strong><span class="classifier">params</span></dt><dd><p>a cometspy.params containing specified simulation parameters</p>
</dd>
<dt><strong>relative_dir</strong><span class="classifier">str, optional</span></dt><dd><p>a directory to place temporary simulation files.</p>
</dd>
</dl>
</td>
</tr>
</tbody>
</table>
<p class="rubric">Examples</p>
<div class="doctest highlight-default notranslate"><div class="highlight"><pre><span></span><span class="gp">&gt;&gt;&gt; </span><span class="c1"># assume layout and params have already been created</span>
<span class="gp">&gt;&gt;&gt; </span><span class="c1"># and cometspy was imported as c</span>
<span class="gp">&gt;&gt;&gt; </span><span class="n">sim</span> <span class="o">=</span> <span class="n">c</span><span class="o">.</span><span class="n">comets</span><span class="p">(</span><span class="n">layout</span><span class="p">,</span> <span class="n">params</span><span class="p">)</span>
<span class="gp">&gt;&gt;&gt; </span><span class="n">sim</span><span class="o">.</span><span class="n">run</span><span class="p">()</span> <span class="c1"># this could take from seconds to hours</span>
<span class="gp">&gt;&gt;&gt; </span><span class="nb">print</span><span class="p">(</span><span class="n">sim</span><span class="o">.</span><span class="n">run_output</span><span class="p">)</span> <span class="c1"># to see the std_out</span>
<span class="gp">&gt;&gt;&gt; </span><span class="n">sim</span><span class="o">.</span><span class="n">total_biomass</span><span class="o">.</span><span class="n">plot</span><span class="p">(</span><span class="n">x</span> <span class="o">=</span> <span class="s2">&quot;cycle&quot;</span><span class="p">)</span>
<span class="gp">&gt;&gt;&gt; </span><span class="c1"># assume params.all_params[&quot;writeBiomassLog&quot;] == True, and that</span>
<span class="gp">&gt;&gt;&gt; </span><span class="c1"># params.all_params[&quot;BiomassLogRate&quot;] = 1000, and that</span>
<span class="gp">&gt;&gt;&gt; </span><span class="c1"># params.all_params[&quot;maxCycles&quot;] = 5000</span>
<span class="gp">&gt;&gt;&gt; </span><span class="n">im</span> <span class="o">=</span> <span class="n">sim</span><span class="o">.</span><span class="n">get_biomass_image</span><span class="p">(</span><span class="n">layout</span><span class="o">.</span><span class="n">models</span><span class="p">[</span><span class="mi">0</span><span class="p">]</span><span class="o">.</span><span class="n">id</span><span class="p">,</span> <span class="mi">4000</span><span class="p">)</span>
<span class="gp">&gt;&gt;&gt; </span><span class="kn">from</span> <span class="nn">matplotlib</span> <span class="kn">import</span> <span class="n">pyplot</span> <span class="k">as</span> <span class="n">plt</span>
<span class="gp">&gt;&gt;&gt; </span><span class="n">plt</span><span class="o">.</span><span class="n">imshow</span><span class="p">(</span><span class="n">im</span> <span class="o">/</span> <span class="n">np</span><span class="o">.</span><span class="n">max</span><span class="p">(</span><span class="n">im</span><span class="p">))</span>
</pre></div>
</div>
<table class="docutils field-list" frame="void" rules="none">
<col class="field-name" />
<col class="field-body" />
<tbody valign="top">
<tr class="field-odd field"><th class="field-name">Attributes:</th><td class="field-body"><dl class="first last docutils">
<dt><strong>layout</strong><span class="classifier">cometspy.layout</span></dt><dd><p>the layout containing cometspy.model[s] and media information</p>
</dd>
<dt><strong>params</strong><span class="classifier">cometspy.params</span></dt><dd><p>the params containing simulation and default biological parameters</p>
</dd>
<dt><strong>working_dir</strong><span class="classifier">str</span></dt><dd><p>the directory at which to save temporary sim files</p>
</dd>
<dt><strong>GUROBI_HOME</strong><span class="classifier">str</span></dt><dd><p>the directory where GUROBI exists on the system</p>
</dd>
<dt><strong>COMETS_HOME</strong><span class="classifier">str</span></dt><dd><p>the directory where COMETS exists on the system</p>
</dd>
<dt><strong>VERSION</strong><span class="classifier">str</span></dt><dd><p>the version of comets, read from the files at COMETS_HOME</p>
</dd>
<dt><strong>classpath_pieces</strong><span class="classifier">dict</span></dt><dd><p>classpath separated into library name (key) and location (value)</p>
</dd>
<dt><strong>JAVA_CLASSPATH</strong><span class="classifier">str</span></dt><dd><p>a generated (overwritable) string containing the java classpath</p>
</dd>
<dt><strong>run_output</strong><span class="classifier">str</span></dt><dd><p>generated object containing text from COMETS sim’s std_out</p>
</dd>
<dt><strong>run_errors</strong><span class="classifier">str</span></dt><dd><p>generated object containing text from COMETS sim’s std_err</p>
</dd>
<dt><strong>total_biomass</strong><span class="classifier">pandas.DataFrame</span></dt><dd><p>generated object containing total biomass from simulation</p>
</dd>
<dt><strong>biomass</strong><span class="classifier">pandas.DataFrame</span></dt><dd><p>generated object containing spatially-explicit biomass from sim</p>
</dd>
<dt><strong>specific_media</strong><span class="classifier">pandas.DataFrame</span></dt><dd><p>generated object containing information on selected media from sim</p>
</dd>
<dt><strong>media</strong><span class="classifier">pandas.Dataframe</span></dt><dd><p>generated object containing spatially-explicit media from sim</p>
</dd>
<dt><strong>fluxes_by_species</strong><span class="classifier">dict{model_id</span></dt><dd><p>generated object containing each species’ spatial-explicit fluxes</p>
</dd>
<dt><strong>genotypes</strong><span class="classifier">pandas.DataFrame</span></dt><dd><p>generated object containing genotypes if an evolution sim was run</p>
</dd>
</dl>
</td>
</tr>
</tbody>
</table>
<p class="rubric">Methods</p>
<table border="1" class="longtable docutils">
<colgroup>
<col width="10%" />
<col width="90%" />
</colgroup>
<tbody valign="top">
<tr class="row-odd"><td><a class="reference internal" href="#cometspy.comets.comets.get_biomass_image" title="cometspy.comets.comets.get_biomass_image"><code class="xref py py-obj docutils literal notranslate"><span class="pre">get_biomass_image</span></code></a>(model_id,&nbsp;cycle)</td>
<td>returns an image of biomass concentrations at a given cycle</td>
</tr>
<tr class="row-even"><td><a class="reference internal" href="#cometspy.comets.comets.get_flux_image" title="cometspy.comets.comets.get_flux_image"><code class="xref py py-obj docutils literal notranslate"><span class="pre">get_flux_image</span></code></a>(model_id,&nbsp;reaction_id,&nbsp;cycle)</td>
<td>returns a 2d numpy array showing fluxes at a given cycle</td>
</tr>
<tr class="row-odd"><td><a class="reference internal" href="#cometspy.comets.comets.get_metabolite_image" title="cometspy.comets.comets.get_metabolite_image"><code class="xref py py-obj docutils literal notranslate"><span class="pre">get_metabolite_image</span></code></a>(met,&nbsp;cycle)</td>
<td>returns an image of metabolite concentrations at a given cycle</td>
</tr>
<tr class="row-even"><td><a class="reference internal" href="#cometspy.comets.comets.run" title="cometspy.comets.comets.run"><code class="xref py py-obj docutils literal notranslate"><span class="pre">run</span></code></a>([delete_files])</td>
<td>run a COMETS simulation</td>
</tr>
<tr class="row-odd"><td><a class="reference internal" href="#cometspy.comets.comets.set_classpath" title="cometspy.comets.comets.set_classpath"><code class="xref py py-obj docutils literal notranslate"><span class="pre">set_classpath</span></code></a>(libraryname,&nbsp;path)</td>
<td>sets the full path to a required specified java library</td>
</tr>
</tbody>
</table>
<dl class="py method">
<dt id="cometspy.comets.comets.get_biomass_image">
<code class="descname">get_biomass_image</code><span class="sig-paren">(</span><em><span class="n">model_id</span><span class="p">:</span> <span class="n">str</span></em>, <em><span class="n">cycle</span><span class="p">:</span> <span class="n">int</span></em><span class="sig-paren">)</span> &#x2192; numpy.array<a class="headerlink" href="#cometspy.comets.comets.get_biomass_image" title="Permalink to this definition">¶</a></dt>
<dd><p>returns an image of biomass concentrations at a given cycle</p>
<p>This will only work if biomass was saved at the current cycle. This 
requires the following parameters to have been set:
&gt;&gt;&gt; params.set_param(“writeBiomassLog”,True)
&gt;&gt;&gt; params.set_param(“BiomassLogRate”, n) # n is in cycles</p>
<table class="docutils field-list" frame="void" rules="none">
<col class="field-name" />
<col class="field-body" />
<tbody valign="top">
<tr class="field-odd field"><th class="field-name">Parameters:</th><td class="field-body"><dl class="first docutils">
<dt><strong>model_id</strong><span class="classifier">str</span></dt><dd><p>the id of the model to get biomass data on</p>
</dd>
<dt><strong>cycle</strong><span class="classifier">int</span></dt><dd><p>the cycle to get the biomass data</p>
</dd>
</dl>
</td>
</tr>
<tr class="field-even field"><th class="field-name">Returns:</th><td class="field-body"><dl class="first last docutils">
<dt>A 2d numpy array which can be visualized like an image.</dt><dd></dd>
</dl>
</td>
</tr>
</tbody>
</table>
<p class="rubric">Examples</p>
<div class="doctest highlight-default notranslate"><div class="highlight"><pre><span></span><span class="gp">&gt;&gt;&gt; </span><span class="n">sim</span><span class="o">.</span><span class="n">run</span><span class="p">()</span>
<span class="gp">&gt;&gt;&gt; </span><span class="c1"># assume sim.params.all_params[&quot;BiomassLogRate&quot;] = 100 and that</span>
<span class="gp">&gt;&gt;&gt; </span><span class="c1"># sim.params.all_params[&quot;maxCycles&quot;] = 1000</span>
<span class="gp">&gt;&gt;&gt; </span><span class="n">im</span> <span class="o">=</span> <span class="n">sim</span><span class="o">.</span><span class="n">get_biomass_image</span><span class="p">(</span><span class="s2">&quot;iJO1366&quot;</span><span class="p">,</span> <span class="mi">500</span><span class="p">)</span> <span class="c1"># e.g. the ecoli model</span>
<span class="gp">&gt;&gt;&gt; </span><span class="kn">from</span> <span class="nn">matplotlib</span> <span class="kn">import</span> <span class="n">pyplot</span> <span class="k">as</span> <span class="n">plt</span> <span class="c1"># may need to be installed</span>
<span class="gp">&gt;&gt;&gt; </span><span class="n">plt</span><span class="o">.</span><span class="n">imshow</span><span class="p">(</span><span class="n">im</span><span class="p">)</span>
</pre></div>
</div>
</dd></dl>

<dl class="py method">
<dt id="cometspy.comets.comets.get_flux_image">
<code class="descname">get_flux_image</code><span class="sig-paren">(</span><em><span class="n">model_id</span><span class="p">:</span> <span class="n">str</span></em>, <em><span class="n">reaction_id</span><span class="p">:</span> <span class="n">str</span></em>, <em><span class="n">cycle</span><span class="p">:</span> <span class="n">int</span></em><span class="sig-paren">)</span> &#x2192; numpy.array<a class="headerlink" href="#cometspy.comets.comets.get_flux_image" title="Permalink to this definition">¶</a></dt>
<dd><p>returns a 2d numpy array showing fluxes at a given cycle</p>
<p>This will only work if flux was saved at the current cycle. This 
requires the following parameters to have been set:</p>
<blockquote>
<div>params.set_param(“writeFluxLog”,True)
params.set_param(“FluxLogRate”, n) # n is in cycles</div></blockquote>
<table class="docutils field-list" frame="void" rules="none">
<col class="field-name" />
<col class="field-body" />
<tbody valign="top">
<tr class="field-odd field"><th class="field-name">Parameters:</th><td class="field-body"><dl class="first docutils">
<dt><strong>model_id</strong><span class="classifier">str</span></dt><dd><p>the id of the model about which to get fluxes</p>
</dd>
<dt><strong>reaction_id</strong><span class="classifier">str</span></dt><dd><p>the id of the reaction about which to get fluxes</p>
</dd>
<dt><strong>cycle</strong><span class="classifier">int</span></dt><dd><p>the cycle at which to get fluxes</p>
</dd>
</dl>
</td>
</tr>
<tr class="field-even field"><th class="field-name">Returns:</th><td class="field-body"><dl class="first last docutils">
<dt>a 2d numpy array which can be visualized like an image</dt><dd></dd>
</dl>
</td>
</tr>
</tbody>
</table>
<p class="rubric">Examples</p>
<div class="doctest highlight-default notranslate"><div class="highlight"><pre><span></span><span class="gp">&gt;&gt;&gt; </span><span class="n">sim</span><span class="o">.</span><span class="n">run</span><span class="p">()</span>
<span class="gp">&gt;&gt;&gt; </span><span class="c1"># assume sim.params.all_params[&quot;FluxLogRate&quot;] = 100 and that</span>
<span class="gp">&gt;&gt;&gt; </span><span class="c1"># sim.params.all_params[&quot;maxCycles&quot;] = 1000</span>
<span class="gp">&gt;&gt;&gt; </span><span class="c1"># assume a model used was iJO1366</span>
<span class="gp">&gt;&gt;&gt; </span><span class="n">im</span> <span class="o">=</span> <span class="n">sim</span><span class="o">.</span><span class="n">get_flux_image</span><span class="p">(</span><span class="s2">&quot;iJO1366&quot;</span><span class="p">,</span> <span class="s2">&quot;EX_ac_e&quot;</span><span class="p">,</span> <span class="mi">500</span><span class="p">)</span> 
<span class="gp">&gt;&gt;&gt; </span><span class="kn">from</span> <span class="nn">matplotlib</span> <span class="kn">import</span> <span class="n">pyplot</span> <span class="k">as</span> <span class="n">plt</span> <span class="c1"># may need to be installed</span>
<span class="gp">&gt;&gt;&gt; </span><span class="n">plt</span><span class="o">.</span><span class="n">imshow</span><span class="p">(</span><span class="n">im</span><span class="p">)</span>
</pre></div>
</div>
</dd></dl>

<dl class="py method">
<dt id="cometspy.comets.comets.get_metabolite_image">
<code class="descname">get_metabolite_image</code><span class="sig-paren">(</span><em><span class="n">met</span><span class="p">:</span> <span class="n">str</span></em>, <em><span class="n">cycle</span><span class="p">:</span> <span class="n">int</span></em><span class="sig-paren">)</span> &#x2192; numpy.array<a class="headerlink" href="#cometspy.comets.comets.get_metabolite_image" title="Permalink to this definition">¶</a></dt>
<dd><p>returns an image of metabolite concentrations at a given cycle</p>
<p>This will only work if media was saved at the current cycle. This 
requires the following parameters to have been set:</p>
<blockquote>
<div>params.set_param(“writeMediaLog”,True)
params.set_param(“MediaLogRate”, n) # n is in cycles</div></blockquote>
<table class="docutils field-list" frame="void" rules="none">
<col class="field-name" />
<col class="field-body" />
<tbody valign="top">
<tr class="field-odd field"><th class="field-name">Parameters:</th><td class="field-body"><dl class="first docutils">
<dt><strong>met</strong><span class="classifier">str</span></dt><dd><p>the name of the metabolite</p>
</dd>
<dt><strong>cycle</strong><span class="classifier">int</span></dt><dd><p>the cycle to get the metabolite data</p>
</dd>
</dl>
</td>
</tr>
<tr class="field-even field"><th class="field-name">Returns:</th><td class="field-body"><dl class="first last docutils">
<dt>A 2d numpy array which can be visualized like an image.</dt><dd></dd>
</dl>
</td>
</tr>
</tbody>
</table>
<p class="rubric">Notes</p>
<p>There may be a bug where cycles are + 1 what they should be. We will
fix this soon but for now be aware you may need to +1 your desired
cycle. The bug does not affect anything within the simulation.</p>
<p class="rubric">Examples</p>
<div class="doctest highlight-default notranslate"><div class="highlight"><pre><span></span><span class="gp">&gt;&gt;&gt; </span><span class="n">sim</span><span class="o">.</span><span class="n">run</span><span class="p">()</span>
<span class="gp">&gt;&gt;&gt; </span><span class="c1"># assume sim.params.all_params[&quot;MediaLogRate&quot;] = 100 and that</span>
<span class="gp">&gt;&gt;&gt; </span><span class="c1"># sim.params.all_params[&quot;maxCycles&quot;] = 1000</span>
<span class="gp">&gt;&gt;&gt; </span><span class="n">im</span> <span class="o">=</span> <span class="n">sim</span><span class="o">.</span><span class="n">get_metabolite_image</span><span class="p">(</span><span class="s2">&quot;ac_e&quot;</span><span class="p">,</span> <span class="mi">500</span><span class="p">)</span>
<span class="gp">&gt;&gt;&gt; </span><span class="kn">from</span> <span class="nn">matplotlib</span> <span class="kn">import</span> <span class="n">pyplot</span> <span class="k">as</span> <span class="n">plt</span> <span class="c1"># may need to be installed</span>
<span class="gp">&gt;&gt;&gt; </span><span class="n">plt</span><span class="o">.</span><span class="n">imshow</span><span class="p">(</span><span class="n">im</span><span class="p">)</span>
</pre></div>
</div>
</dd></dl>

<dl class="py method">
<dt id="cometspy.comets.comets.run">
<code class="descname">run</code><span class="sig-paren">(</span><em><span class="n">delete_files</span><span class="p">:</span> <span class="n">bool</span> <span class="o">=</span> <span class="default_value">True</span></em><span class="sig-paren">)</span><a class="headerlink" href="#cometspy.comets.comets.run" title="Permalink to this definition">¶</a></dt>
<dd><p>run a COMETS simulation</p>
<p>This runs the COMETS simulation specified by the layout and params
objects supplied to this comets object. It creates the files needed
for COMETS into the current directory or the optional one specified
when this object was created.</p>
<p>Once complete (or if an error has occurred), this tries to read 
simulation logs including data, as well as the std_out in the
run_output attribute.</p>
<p>If the optional delete_files is set to False, then temporary files and 
data log files are not deleted. They are deleted by default.</p>
<table class="docutils field-list" frame="void" rules="none">
<col class="field-name" />
<col class="field-body" />
<tbody valign="top">
<tr class="field-odd field"><th class="field-name">Parameters:</th><td class="field-body"><dl class="first last docutils">
<dt><strong>delete_files</strong><span class="classifier">bool, optional</span></dt><dd><p>Whether to delete simulation and log files. The default is True.</p>
</dd>
</dl>
</td>
</tr>
</tbody>
</table>
<p class="rubric">Examples</p>
<div class="doctest highlight-default notranslate"><div class="highlight"><pre><span></span><span class="gp">&gt;&gt;&gt; </span><span class="n">sim</span> <span class="o">=</span> <span class="n">c</span><span class="o">.</span><span class="n">comets</span><span class="p">(</span><span class="n">layout</span><span class="p">,</span> <span class="n">params</span><span class="p">)</span>
<span class="gp">&gt;&gt;&gt; </span><span class="n">sim</span><span class="o">.</span><span class="n">run</span><span class="p">(</span><span class="n">delete_files</span> <span class="o">=</span> <span class="kc">True</span><span class="p">)</span>
<span class="gp">&gt;&gt;&gt; </span><span class="nb">print</span><span class="p">(</span><span class="n">sim</span><span class="o">.</span><span class="n">run_output</span><span class="p">)</span>
<span class="gp">&gt;&gt;&gt; </span><span class="nb">print</span><span class="p">(</span><span class="n">sim</span><span class="o">.</span><span class="n">total_biomass</span><span class="p">)</span>
</pre></div>
</div>
</dd></dl>

<dl class="py method">
<dt id="cometspy.comets.comets.set_classpath">
<code class="descname">set_classpath</code><span class="sig-paren">(</span><em><span class="n">libraryname</span><span class="p">:</span> <span class="n">str</span></em>, <em><span class="n">path</span><span class="p">:</span> <span class="n">str</span></em><span class="sig-paren">)</span><a class="headerlink" href="#cometspy.comets.comets.set_classpath" title="Permalink to this definition">¶</a></dt>
<dd><p>sets the full path to a required specified java library</p>
<p>This can be used to set non-default classpaths. Note that currently,
it does not work for windows, because windows runs COMETS slightly
differently than Unix.</p>
<table class="docutils field-list" frame="void" rules="none">
<col class="field-name" />
<col class="field-body" />
<tbody valign="top">
<tr class="field-odd field"><th class="field-name">Parameters:</th><td class="field-body"><dl class="first last docutils">
<dt><strong>libraryname</strong><span class="classifier">str</span></dt><dd><p>the library for which the new path is being supplied.</p>
</dd>
<dt><strong>path</strong><span class="classifier">str</span></dt><dd><p>the full path, including file name, to the library</p>
</dd>
</dl>
</td>
</tr>
</tbody>
</table>
<p class="rubric">Examples</p>
<div class="doctest highlight-default notranslate"><div class="highlight"><pre><span></span><span class="gp">&gt;&gt;&gt; </span><span class="n">sim</span> <span class="o">=</span> <span class="n">c</span><span class="o">.</span><span class="n">comets</span><span class="p">(</span><span class="n">layout</span><span class="p">,</span> <span class="n">params</span><span class="p">)</span>
<span class="gp">&gt;&gt;&gt; </span><span class="n">sim</span><span class="o">.</span><span class="n">set_classpath</span><span class="p">(</span><span class="s2">&quot;jmatio&quot;</span><span class="p">,</span> <span class="s2">&quot;/opt/jmatio/jmatio.jar&quot;</span><span class="p">)</span>
</pre></div>
</div>
</dd></dl>

</dd></dl>

</div>


            <div class="clearer"></div>
          </div>
        </div>
      </div>
      <div class="sphinxsidebar" role="navigation" aria-label="main navigation">
        <div class="sphinxsidebarwrapper">
<h1 class="logo"><a href="index.html">cometspy</a></h1>





<p>
<iframe src="https://ghbtns.com/github-btn.html?user=&repo=&type=watch&count=true&size=large"
  allowtransparency="true" frameborder="0" scrolling="0" width="200px" height="35px"></iframe>
</p>




<h3>Navigation</h3>
<p class="caption"><span class="caption-text">Contents:</span></p>
<ul class="current">
<li class="toctree-l1 current"><a class="reference internal" href="cometspy.html">cometspy:</a><ul class="current">
<li class="toctree-l2 current"><a class="reference internal" href="cometspy.html#submodules">Submodules</a></li>
</ul>
</li>
</ul>

<div class="relations">
<h3>Related Topics</h3>
<ul>
  <li><a href="index.html">Documentation overview</a><ul>
  <li><a href="cometspy.html">cometspy:</a><ul>
      <li>Previous: <a href="cometspy.html" title="previous chapter">cometspy:</a></li>
      <li>Next: <a href="cometspy.layout.html" title="next chapter">cometspy.layout module</a></li>
  </ul></li>
  </ul></li>
</ul>
</div>
<div id="searchbox" style="display: none" role="search">
  <h3 id="searchlabel">Quick search</h3>
    <div class="searchformwrapper">
    <form class="search" action="search.html" method="get">
      <input type="text" name="q" aria-labelledby="searchlabel" />
      <input type="submit" value="Go" />
    </form>
    </div>
</div>
<script>$('#searchbox').show(0);</script>
        </div>
      </div>
      <div class="clearer"></div>
    </div>
    <div class="footer">
      &copy;2020, D Bajic, J Chacon, J Vila.
      
      |
      Powered by <a href="http://sphinx-doc.org/">Sphinx 3.3.1</a>
      &amp; <a href="https://github.com/bitprophet/alabaster">Alabaster 0.7.7</a>
      
      |
      <a href="_sources/cometspy.comets.rst.txt"
          rel="nofollow">Page source</a>
    </div>

    

    
  </body>
</html>