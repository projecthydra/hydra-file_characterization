# hydra-file_chracterization

Hydra::FileCharacterization as (extracted from Sufia and Hydra::Derivatives)

## Purpose

To provide a wrapper for file characterization

## How To Use

If you are using Rails add the following to an initializer (./config/initializers/hydra-file_characterization_config.rb):

```ruby
Hydra::FileCharacterization.configure do |config|
  config.tool_path(:fits, '/path/to/fits')
end
```

You can call a single characterizer…
```ruby
xml_string = Hydra::FileCharacterization.characterize(contents_of_a_file, 'file.rb', :fits)
```

…for this particular call, you can specify custom fits path…

```ruby
xml_string = Hydra::FileCharacterization.characterize(contents_of_a_file, 'file.rb', :fits) do |config|
  config[:fits] = './really/custom/path/to/fits'
end
```

…or even make the path callable

```ruby
xml_string = Hydra::FileCharacterization.characterize(contents_of_a_file, 'file.rb', :fits) do |config|
  config[:fits] = lambda {|filename| … }
end
```

You can also call multiple characterizers at the same time.

```ruby
fits_xml, ffprobe_xml = Hydra::FileCharacterization.characterize(contents_of_a_file, 'file.rb', :fits, :ffprobe)
```

* Why `file.read`? To highlight that we want a string. In the case of ActiveFedora, we have a StringIO instead of a file.
* Why `file.basename`? In the case of Fits, the characterization takes cues from the extension name.

## Registering New Characterizers

This is possible by adding a characterizer to the `Hydra::FileCharacterization::Characterizers`' namespace.
