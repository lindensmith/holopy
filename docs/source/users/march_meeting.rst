.. _march_meeting:

NOT TO BE USED FOR ANYTHING - THIS IS A TEST
============================================

I like the way readthedocs formats code, so I'm putting some code up temporarily to use in my poster.

..  testcode::

    import holopy as hp
    my_hologram = hp.load_image('holo.tif',spacing = 0.0858, illum_wavelen = 0.52, medium_index = 1.33)
    hp.show(my_hologram)

loaded an image and metadata.

..  testcode::
    
    z_distances = [0, 8, 16]
    reconstructions = hp.propagate(my_hologram, z_distances)
    hp.show(reconstructions)

Do reconstructions to a few distances

..  testcode::

    from holopy.scattering import Sphere, Spheres, calc_holo
    my_centers = [[14.87,16,16], [16,15.35,16], [16,16.65,16]]
    my_scatterer = Spheres([Sphere(r=.65, n=1.59, center=my_centers[i]) for i in range(3)])
    calculated_hologram = calc_holo(my_hologram, my_scatterer)
    hp.show(calculated_hologram)


calculate hologram from a trimer

..  testcode::

    from holopy.inference import prior, AlphaModel#, EmceeStrategy
    cluster_center = [16.0, 15.9, 17.5]
    proposed_scatterer = Spheres([Sphere(r=.65, n=1.59, center=[prior.Uniform(cluster_center[i]-1,cluster_center[i]+1)
                                                            for i in range(3)]) for j in range(3)])

    my_model = AlphaModel(proposed_scatterer, noise_sd=my_hologram.std(), alpha=0.77, theory=Multisphere(suppress_fortran_output=False))
    my_strategy = EmceeStrategy(nwalkers=2000, pixels=250)
    best_scatterer = my_strategy.sample(my_model, my_hologram, 100).values()

