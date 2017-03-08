.. _march_meeting:

NOT TO BE USED FOR ANYTHING - THIS IS A TEST
============================================

I like the way readthedocs formats code, so I'm putting some code up temporarily to use in my poster.

..  testcode::

    import holopy as hp
    my_holo = hp.load_image('holo.tif', spacing = .0858, illum_wavelen = .52, medium_index = 1.33)
    hp.show(my_holo)

loaded an image and metadata.

..  testcode::
    
    z_distances = [0, 8, 16]
    reconstructions = hp.propagate(my_holo, z_distances)
    hp.show(reconstructions)

Do reconstructions to a few distances

..  testcode::

    from holopy.scattering import Sphere, Spheres, calc_holo
    my_centers = [[14.87,16,16], [16,15.35,16], [16,16.65,16]]
    my_scatterer = Spheres([Sphere(r=.65, n=1.59, center=my_centers[i]) for i in range(3)])
    calculated_holo = calc_holo(my_holo, my_scatterer)
    hp.show(calculated_holo)


calculate hologram from a trimer

..  testcode::

    from holopy.inference import prior, AlphaModel#, EmceeStrategy
    proposed_scatterer = Spheres([Sphere(r=.65, n=1.59, center=[prior.Uniform(16-1.3, 16+1.3)
                                    for i in range(3)]) for j in range(3)])
    my_model = AlphaModel(proposed_scatterer, noise_sd=my_holo.std(), alpha=0.77)
    my_strategy = EmceeStrategy(nwalkers=10000, pixels=250)
    best_scatterer = my_strategy.sample(my_model, my_holo, 100).values()

